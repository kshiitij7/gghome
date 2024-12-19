<template>
<div class="Basin">

    <v-main>
        <!-- Left Side Bar -->
        <v-navigation-drawer rail app color="black">
            <v-list>
                <!-- Layers Button -->
                <v-list-item @click="openLeftDrawer('layers')" class="cursor-pointer">
                    <v-tooltip location="left">
                        <template v-slot:activator="{ props: tooltip }">
                            <v-icon v-bind="tooltip">mdi-layers</v-icon>
                        </template>
                        <span>Layers</span>
                    </v-tooltip>
                </v-list-item>
                <v-divider :thickness="2" color="white"></v-divider>
                <!-- Time Series Button -->
                <v-list-item @click="openLeftDrawer('timeSeries')" class="cursor-pointer">
                    <v-tooltip location="left">
                        <template v-slot:activator="{ props: tooltip }">
                            <v-icon v-bind="tooltip">mdi-chart-timeline</v-icon>
                        </template>
                        <span>Time Series</span>
                    </v-tooltip>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>

        <!-- Dynamic Drawer -->
        <v-navigation-drawer v-model="isLeftDrawerOpen" location="left" :width="250">
            <!-- Layers Content -->
            <template v-if="activeLeftTab === 'layers'">
                <v-list-item>
                    <div class="text-h5" style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center;">Layer Switcher</div>
                </v-list-item>
                <v-divider :thickness="2" color="black"></v-divider>
                <v-title>
                    <div class="text-h7" style="font-family: 'Poppins', sans-serif; font-weight:500; margin-top: 20px; margin-left: 20px;">Base Maps</div>
                </v-title>
                <v-select v-model="baseMaps" :items="baseMapsLayers" label="Please Select a Map" required></v-select>
                <v-title>
                    <div class="text-h7" style="font-family: 'Poppins', sans-serif; font-weight:500; margin-top: 20px; margin-left: 20px;">Administrative Boundaries</div>
                </v-title>
                <v-select style="margin-bottom: 50px;" chips multiple clearable v-model="adminBoundary" :items="adminBoundaryLayers" label="Please Select a Layer" required></v-select>
                <v-divider :thickness="3"></v-divider>
            </template>

            <!-- Time Series Content -->
            <template v-if="activeLeftTab === 'timeSeries'">
                <v-list-item>
                    <div class="text-h5" style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center;">Time Series Data</div>
                </v-list-item>
                <v-divider :thickness="2" color="black"></v-divider>
                <v-select style="margin-top: 40px; margin-bottom: 30px;" v-model="timeSeries" :items="timeSeriesLayers" label="Please Select an Option" required></v-select>
                <v-divider :thickness="3"></v-divider>
            </template>
        </v-navigation-drawer>

        <!-- Map -->
        <div ref="basinRef" style=" height: 86vh;">
            <BasinComponent />
        </div>

        <v-navigation-drawer rail app location="right" color="black">
            <v-list>
                <!-- Search Button -->
                <v-list-item @click="openRightDrawer('search')" class="cursor-pointer">
                    <v-list-item-title>
                        <v-tooltip location="right">
                            <template v-slot:activator="{ props: tooltip }">
                                <v-icon v-bind="tooltip">mdi-magnify</v-icon>
                            </template>
                            <span>Search</span>
                        </v-tooltip>
                    </v-list-item-title>
                </v-list-item>
                <v-divider :thickness="2" color="white"></v-divider>
                <!-- Feature Info Button -->
                <v-list-item @click="openRightDrawer('featureInfo')" class="cursor-pointer">
                    <v-list-item-title>
                        <v-tooltip location="right">
                            <template v-slot:activator="{ props: tooltip }">
                                <v-icon v-bind="tooltip">mdi-information</v-icon>
                            </template>
                            <span>Feature Info</span>
                        </v-tooltip>
                    </v-list-item-title>
                </v-list-item>
                <v-divider :thickness="2" color="white"></v-divider>
                <!-- Measure Button -->
                <v-list-item @click="openRightDrawer('measurement')" class="cursor-pointer">
                    <v-list-item-title>
                        <v-tooltip location="right">
                            <template v-slot:activator="{ props: tooltip }">
                                <v-icon v-bind="tooltip">mdi-ruler-square</v-icon>
                            </template>
                            <span>Measurement Tool</span>
                        </v-tooltip>
                    </v-list-item-title>
                </v-list-item>
                <v-divider :thickness="2" color="white"></v-divider>
                <!-- Compare Button -->
                <v-list-item @click="openRightDrawer('compare')" class="cursor-pointer">
                    <v-list-item-title>
                        <v-tooltip location="right">
                            <template v-slot:activator="{ props: tooltip }">
                                <v-icon v-bind="tooltip">mdi-compare</v-icon>
                            </template>
                            <span>Compare Tool</span>
                        </v-tooltip>
                    </v-list-item-title>
                </v-list-item>
            </v-list>
        </v-navigation-drawer>

        <!-- Dynamic Drawer -->
        <v-navigation-drawer v-model="isRightDrawerOpen" location="right" :width="300">
            <!-- Render Content Dynamically Based on `activeTool` -->
            <div v-if="activeTool === 'search'">
                <v-card elevation="0">
                    <v-card-title>
                        <div class="text-h5" style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center;">Search for a place</div>
                    </v-card-title>
                    <v-divider :thickness="2" color="black"></v-divider>
                    <v-card-text>
                        <v-text-field v-model="searchQuery" @searchInput="onSearchInput" style="margin-top: 20px; margin-bottom: 30px;" clearable placeholder="Type to Search ..." outlined></v-text-field>
                    </v-card-text>
                </v-card>
                <v-divider :thickness="3"></v-divider>
                <v-img src="../GangaBasin/rightPng/search_button.png" style="margin-top: 100px; opacity: 0.8;" contain alt="Comparison Tool Image"></v-img>
            </div>

            <div v-else-if="activeTool === 'featureInfo'">
                <v-card elevation="0">
                    <v-card-title>
                        <div class="text-h5" style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center;">Feature Info Tool</div>
                    </v-card-title>
                    <v-divider :thickness="2" color="black"></v-divider>
                    <v-card-text>
                        <v-switch color="blue" label="Enable Feature Info for Vector"></v-switch>
                        <v-divider></v-divider>
                        <v-switch style="margin-top: 20px;" color="purple" label="Enable Feature Info for Raster"></v-switch>
                    </v-card-text>
                </v-card>
                <v-divider :thickness="3"></v-divider>
                <v-img src="../GangaBasin/rightPng/feature_tool.png" style="margin-top: 70px;opacity: 0.8;" contain alt="Comparison Tool Image"></v-img>
            </div>

            <div v-else-if="activeTool === 'measurement'">
                <v-card elevation="0">
                    <v-card-title>
                        <div class="text-h5" style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center;">Measurement Tool</div>
                    </v-card-title>
                    <v-divider :thickness="2" color="black"></v-divider>
                    <v-card-text style="margin-bottom: 30px; margin-top: 10px;">
                        <v-container class="icon-container" justify="center">
                            <v-row justify="center">
                                <v-col cols="4" class="text-center">
                                    <v-btn :color="activeMeasurement === 'Length' ? 'blue' : ''" prepend-icon="mdi-pulse" stacked @click="activateMeasurement('Length')">Length</v-btn>
                                </v-col>
                                <v-col cols="4" class="text-center">
                                    <v-btn :color="activeMeasurement === 'Area' ? 'blue' : ''" prepend-icon="mdi-pentagon-outline" stacked @click="activateMeasurement('Area')">Area</v-btn>
                                </v-col>
                                <v-col cols="4" class="text-center">
                                    <v-btn color="#700B0B" prepend-icon="mdi-delete" stacked @click="clearMeasurements">Clear </v-btn>
                                </v-col>
                            </v-row>
                        </v-container>
                    </v-card-text>
                </v-card>
                <v-divider :thickness="3"></v-divider>
                <v-img src="../GangaBasin/rightPng/measure_tool.png" style="margin-top: 50px;opacity: 0.8;" contain alt="Comparison Tool Image"></v-img>
            </div>

            <div v-else-if="activeTool === 'compare'">
                <v-card elevation="0">
                    <v-card-title>
                        <div  class="text-h5" style="font-family: 'Poppins', sans-serif; font-weight:500; text-align: center;">Comparison Tool</div>
                    </v-card-title>
                    <v-divider :thickness="2" color="black"></v-divider>
                    <v-card-text>
                        <v-title>
                            <div class="text-h8 font-weight-bold">Left Side</div>
                        </v-title>
                        <v-select v-model="leftSelect" :items="leftMonths" label="Please Select a Layer"></v-select>
                        <v-title>
                            <div class="text-h8 font-weight-bold">Right Side</div>
                        </v-title>
                        <v-select v-model="rightSelect" :items="rightMonths" label="Please Select a Layer"></v-select>
                        <v-btn :disabled="!isBothSelected || showError" @click="handleCompare">Compare</v-btn>
                        <div v-if="showError" class="text-red text-caption mt-2">Please select different months.</div>
                    </v-card-text>
                </v-card>
                <v-divider :thickness="3"></v-divider>
                <v-img src="../GangaBasin/rightPng/compare_tool.png" style="margin-top: 80px; opacity: 0.9;" max-height="300" contain alt="Comparison Tool Image"></v-img>
            </div>
        </v-navigation-drawer>

    </v-main>
</div>
</template>

<script>
import BasinComponent from '../GangaBasin/BasinComponent.vue';
import eventBus from '@/event-bus';

export default {
    name: 'BasinPage',
    components: {
        BasinComponent,
    },
    data() {
        return {
            // Left 
            isLeftDrawerOpen: false,
            activeLeftTab: null,
            baseMaps: null,
            adminBoundary: null,
            timeSeries: null,
            baseMapsLayers: ['Bing', 'Open Street Map', 'Bhuvan Satellite Imagery'],
            adminBoundaryLayers: ['India Boundary','Ganga Basin', 'State Boundary', 'District Boundary', 'Sub-District Boundary'],
            timeSeriesLayers: ['Evapotranspiration', 'Precipitation', 'Temperature'],

            // Right 
            isRightDrawerOpen: false,
            activeTool: null,
            searchQuery: '',
            activeMeasurement: null,
            leftSelect: null,
            rightSelect: null,
            leftMonths: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
            rightMonths: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],

        };
    },
    computed: {
        isBothSelected() {
            return this.leftSelect && this.rightSelect;
        },
        showError() {
            return this.leftSelect === this.rightSelect;
        },
    },
    methods: {
        openLeftDrawer(leftTab) {
            if(this.activeLeftTab === leftTab && this.isLeftDrawerOpen){
                this.isLeftDrawerOpen = false;
                this.activeLeftTab = null;
            } else {
                this.activeLeftTab = leftTab;
                this.isLeftDrawerOpen = true;
            }
        },
        openRightDrawer(tool) {
            if (this.activeTool === tool && this.isRightDrawerOpen) { 
                this.isRightDrawerOpen = false;
                this.activeTool = null;
            } else { 
                this.activeTool = tool;
                this.isRightDrawerOpen = true;
            }
        },
        activateMeasurement(type) {
            this.activeMeasurement = type;
            eventBus.emit('set-measurement-mode', type);
        },
        clearMeasurements() {
            this.activeMeasurement = null;
            eventBus.emit('clear-measurements');
        },
        handleCompare() {
            if (!this.showError) {
                alert('Comparison started!');
            }
        },

    },
};
</script>
