<template>
<div ref="map" class="map" style=" width: 100%;height: 100%;"></div>
<div id="mouse-position" style="position: absolute;bottom: 5.7%; left: 9%; background-color: rgba(0, 0, 0, 0.7);color: white; padding: 0.1em 0.1em; border-radius: 4px; font-family: 'Poppins', sans-serif ; font-size: calc(5px + 0.5vw); z-index: 1000; pointer-events: none; max-width: 30%; text-align: center;"></div>
</template>

<script>
import 'ol/ol.css';
import { Map } from 'ol';
import BingMaps from 'ol/source/BingMaps';
import View from 'ol/View';
import TileLayer from 'ol/layer/Tile';
import OSM from 'ol/source/OSM';
import LayerSwitcher from 'ol-layerswitcher';
import 'ol-layerswitcher/dist/ol-layerswitcher.css';
import TileWMS from 'ol/source/TileWMS';
import { get as getProjection} from 'ol/proj';
import FullScreen from 'ol/control/FullScreen';
import {Draw,} from 'ol/interaction';
import { Vector as VectorSource} from 'ol/source';
import {Vector as VectorLayer} from 'ol/layer';
import {MousePosition} from 'ol/control'; 
import {ScaleLine,defaults as defaultControls} from 'ol/control.js';
import {createStringXY} from 'ol/coordinate';
import eventBus from '@/event-bus';
// import { fromLonLat } from 'ol/proj';
import { getLength, getArea } from 'ol/sphere';

export default {
    name: 'BasinComponent',
    mounted() {
        const osmLayer = new TileLayer({
            title: 'Open Street Map',
            type: 'overlay',
            source: new OSM(),
            visible: false,
        });
        const bingLayer = new TileLayer({
            title: 'Bing',
            type: 'overlay',
            source: new BingMaps({
                key: "ArIdKOW0eb8TRcLZdt0l2cG8kHA_uW92yIvx1aFzsQ1xHxpnVRMGmO0N0neY8P90",
                imagerySet: 'AerialWithLabels',
            }),
            visible: true,
        });
        const bhuvanLayer = new TileLayer({
            title: 'Bhuvan',
            type: 'overlay',
            source: new TileWMS({
                params: {
                    FORMAT: "image/jpeg",
                    VERSION: "1.1.1",
                    tiled: true,
                    LAYERS: "bhuvan_imagery",
                    exceptions: "application/vnd.ogc.se_inimage",
                },
                url: "https://bhuvan-ras2.nrsc.gov.in/tilecache/tilecache.py",
            }),
            visible: false,
        });
        const indiaCountryBoundary = new TileLayer({
            title: 'India Boundary',
            type: 'overlay',
            source: new TileWMS({
                url: 'http://192.168.17.37:8080/geoserver/Geo-Ganga/wms?',
                params: {
                    'LAYERS': 'india_country_boundary_4326',
                    'TILED': true,
                    'VERSION': '1.1.1',
                },
                serverType: 'geoserver',
                tileGrid: new TileWMS().getTileGridForProjection(getProjection('EPSG:4326')),
            }),
            visible: true,
        });
        const basinBoundary = new TileLayer({
            title: 'Ganga Basin',
            type: 'overlay',
            source: new TileWMS({
                url: 'http://192.168.17.37:8080/geoserver/Geo-Ganga/wms?',
                params: {
                    'LAYERS': 'Ganga_Basin_v4',
                    'TILED': true,
                    'VERSION': '1.1.1',
                },
                serverType: 'geoserver',
                tileGrid: new TileWMS().getTileGridForProjection(getProjection('EPSG:4326')),
            }),
            visible: true,
        });
        const basinStatesBoundary = new TileLayer({
            title: 'State Boundary',
            type: 'overlay',
            source: new TileWMS({
                url: 'http://192.168.17.37:8080/geoserver/Geo-Ganga/wms?',
                params: {
                    'LAYERS': 'states_boundary_ganga',
                    'TILED': true,
                    'VERSION': '1.1.1',
                },
                serverType: 'geoserver',
                tileGrid: new TileWMS().getTileGridForProjection(getProjection('EPSG:4326')),
            }),
            visible: false,
        });
        const basinDistrictsBoundary = new TileLayer({
            title: 'District Boundary',
            type: 'overlay',
            source: new TileWMS({
                url: 'http://192.168.17.37:8080/geoserver/Geo-Ganga/wms?',
                params: {
                    'LAYERS': 'district_touch',
                    'TILED': true,
                    'VERSION': '1.1.1',
                },
                serverType: 'geoserver',
                tileGrid: new TileWMS().getTileGridForProjection(getProjection('EPSG:4326')),
            }),
            visible: false,
        });
        const basinSubDistrictsBoundary = new TileLayer({
            title: 'Sub-District Boundary',
            type: 'overlay',
            source: new TileWMS({
                url: 'http://192.168.17.37:8080/geoserver/Geo-Ganga/wms?',
                params: {
                    'LAYERS': 'subdistrict_touch',
                    'TILED': true,
                    'VERSION': '1.1.1',
                },
                serverType: 'geoserver',
                tileGrid: new TileWMS().getTileGridForProjection(getProjection('EPSG:4326')),
            }),
            visible: false,
        });

        const scaleControl = new ScaleLine({
            units: 'metric',
            bar: true,
            steps: 4,
            text: true,
            minWidth: 140,
        });

        const map = new Map({
            controls: defaultControls().extend([scaleControl]),
            target: this.$refs.map,
            layers: [bhuvanLayer, osmLayer, bingLayer, basinSubDistrictsBoundary, basinDistrictsBoundary, basinStatesBoundary, indiaCountryBoundary, basinBoundary],
            view: new View({
                projection: 'EPSG:4326',
                center: [82.0662, 26.2648],
                zoom: 6.5,
                minZoom: 6.5,
            }),
        });

        const mousePositionControl = new MousePosition({projection: 'EPSG:4326', coordinateFormat: createStringXY(4), target: document.getElementById('mouse-position'), className: '', });
        map.addControl(mousePositionControl);

        // Measurement Layer
        this.measurementSource = new VectorSource();
        this.measurementLayer = new VectorLayer({
            source: this.measurementSource,
            style: {
                'fill-color': 'rgba(255, 58, 51, 0.25)',
                'stroke-color': 'yellow',
                'stroke-width': 2,
            },
        });
        map.addLayer(this.measurementLayer);

        // Listen to events from BasinPage to activate and set measurement mode
        eventBus.on('set-measurement-mode', this.setMeasurementMode);
        eventBus.on('clear-measurements', this.deactivateMeasurement);
        eventBus.on('clear-measurements', this.clearMeasurements);

        // LayerSwitcher
        const layerSwitcher = new LayerSwitcher({
            activationMode: 'click',
            startActive: false,
            tipLabel: 'Layers',
            collapseLabel: '\u00BB',
            expandLabel: '\u00AB',
            showRoot: false,
        });
        map.addControl(layerSwitcher);

        // FullScreen 
        const fullScreenControl = new FullScreen({
            tipLabel: 'Fullscreen',
        });
        map.addControl(fullScreenControl);

        this.map = map; // Store map instance
    },

    methods: {

        setMeasurementMode(mode) {
            this.deactivateMeasurement();
            this.measurementMode = mode;
            this.activateMeasurement(mode);
        },
        activateMeasurement(mode) {
        const type = mode === 'Length' ? 'LineString' : 'Polygon';
        this.drawInteraction = new Draw({
            source: this.measurementSource,
            type: type,
        });
        this.drawInteraction.on('drawend', (event) => {
            const feature = event.feature;
            const geometry = feature.getGeometry();
            if (mode === 'Length') {
                const length = getLength(geometry);
                alert(`Length: ${length*100} km`);
            } else if (mode === 'Area') {
                const area = getArea(geometry);
                alert(`Area: ${area*1000} km²`);
            }
        });
        this.map.addInteraction(this.drawInteraction);
    },

        deactivateMeasurement() {
            if (this.drawInteraction) {
                this.map.removeInteraction(this.drawInteraction);
            }
            if (this.modifyInteraction) {
                this.map.removeInteraction(this.modifyInteraction);
            }
        },
        clearMeasurements() {
            this.measurementSource.clear();
        },
    },
};
</script>

<style scoped>
</style>
