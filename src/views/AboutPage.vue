<template>
<v-main>

    <h1 class="heading">About Geo-Ganga</h1>

    <v-row style="margin-left: 40px; margin-right: 40px; margin-top: 20px;">   
        <v-col>
        <p style="font-size: 18px; text-align: justify; font-family: 'Poppins', sans-serif; font-weight: 300;">The Geo-Ganga portal is an innovative digital platform
            to support the initiatives for the rejuvenation and conservation of the Ganga River and its basin.
            This portal provides geospatial data, satellite imagery, and mapping tools that are essential
            for effective monitoring, management, and decision-making related to the Ganga's ecosystem.
            The platform integrates data from various sources, including remote sensing technologies,
            GIS (Geographic Information Systems), and field surveys, offering a comprehensive view of the river and its surrounding areas.
            Geo-Ganga aims to aid in the planning, implementation, and monitoring of the Namami Gange Programme,
            a flagship initiative to clean and protect the Ganga River. It provides crucial insights for assessing water quality,
            identifying pollution hotspots, tracking the progress of river rejuvenation projects, and ensuring sustainable development along the river.
            Through the portal, users can access various layers of information, including land use patterns,
            pollution sources, and infrastructure data. It also helps stakeholders, including government agencies,
            researchers, and environmental organizations, to collaborate effectively
            in Ganga conservation efforts. In sum, Geo-Ganga plays a vital role in enhancing
            the efficiency and transparency of efforts to restore and protect one of India’s most iconic rivers.</p>
        </v-col>
        <v-col style="max-width: 450px; max-height: 450px;">
            <v-img :src="river" style=" width: 100%; height: auto;"></v-img>
        </v-col>
    </v-row>


    <v-row style="margin-left: 40px; margin-top: 80px; margin-right: 40px;">
        <v-col style="max-width: 600px; max-height: 600px;">
            <v-img :src="basinimg" class="aboutimg"></v-img>
        </v-col>
        <v-col>
            <div class="about-text">
                <h2 style="font-size: 27px;">1. Ganga Basin</h2>
                <br>
                <p style="font-size:18px; text-align: justify;">The Ganges Basin, part of the Ganges-Brahmaputra-Meghna (GBM) river system,
                    covers about 1,999,000 square kilometers across Tibet, Nepal, India, and Bangladesh.
                    It is surrounded by the Himalayas to the north, the Indus Basin and Aravalli ridge to the west,
                    the Vindhya Mountain range and Chota Nagpur Plateau to the south, and merges with the Brahmaputra Basin in the east.
                    This vast area includes the Indian states of Uttar Pradesh, Madhya Pradesh, Bihar, Rajasthan, West Bengal, Haryana,
                    Himachal Pradesh, Delhi, and Arunachal Pradesh, along with Nepal, Bhutan, and Bangladesh.
                    The basin supports a population of over 500 million people, making it the most populated river basin in the world.
                    It is drained by several tributaries, many of which originate in Tibet and flow through Nepal before joining the Ganges River.
                    These tributaries provide essential water for agriculture and daily life.
                    The Ganges River system is vital for agriculture in the region,
                    with its fertile plains supporting the production of crops like rice and wheat.
                    However, the basin faces significant challenges such as pollution, water scarcity,
                    and the impacts of climate change, including flooding and reduced snowmelt in the Himalayas.
                    Despite these issues, the Ganges Basin remains crucial to the economy, culture,
                    and religious practices of the millions who depend on its resources.
                    Its role in sustaining biodiversity and providing livelihoods makes it one of the most important and complex river systems in the world.</p>
            </div>
        </v-col>
    </v-row>

    <v-row style="margin-left: 40px; margin-top: 80px; margin-right: 40px;">
        <v-col style="max-width: 600px; max-height: 600px;">
            <v-img :src="qualityimg" class="aboutimg"></v-img>
        </v-col>
        <v-col>
            <div class="about-text">
                <h2 style="font-size: 27px;">2. Water Quality</h2>
                <br>
                <p style="font-size:18px; text-align: justify;">The water quality of the Ganga River is a complex issue,
                    influenced by a variety of environmental, industrial, and social factors.
                    The river, which flows through densely populated regions of India, faces severe pollution in many of its stretches.
                    In the upper reaches, particularly in the Himalayan region, the water quality is generally better, with clear, cold water
                    and high levels of dissolved oxygen, supporting a diverse aquatic ecosystem.
                    However, as the river moves through urban and industrial centers like Kanpur, Varanasi, and Allahabad,
                    its water quality deteriorates significantly. Industrial effluents, including toxic chemicals and heavy metals,
                    are often released directly into the river without adequate treatment.
                    Additionally, untreated sewage from millions of people, as well as runoff from
                    agriculture that carries pesticides and fertilizers, further pollutes the water.
                    These contaminants contribute to high levels of biochemical oxygen demand (BOD),
                    making the water unsafe for drinking and harmful to aquatic life.
                    Religious practices, such as bathing, immersion of idols, and offering flowers,
                    also add to the organic waste and litter in the river. The presence of coliform bacteria,
                    especially fecal coliforms, is alarmingly high, reflecting the unsafe levels of sanitation along the river.
                    In response, the Indian government has launched initiatives like the Namami Gange program,
                    aimed at cleaning the river through wastewater treatment plants, afforestation, and community awareness.
                    However, the Ganga’s water quality continues to be a challenge, requiring concerted efforts in pollution control,
                    infrastructure development, and public participation to restore its ecological health and ensure safe water for millions of people.</p>
            </div>
        </v-col>
    </v-row>
</v-main>
</template>

<script>
export default {
    name: 'AboutPage',
    data() {
        return {
            basinimg: require('@/assets/img/about_img.png'),
            qualityimg: require('@/assets/img/waterquality.png'),
            river: require('@/assets/img/portal.png'),
        };
    },
};
</script>

<style scoped>
.heading {
    font-family: 'Poppins', sans-serif;
    font-weight: 300;
    font-size: 50px;
    text-decoration: underline;
    text-align: center;
    width: 100%;
}

.aboutimg {
    width: 100%;
    height: auto;
}

.about-text {
    padding: 20px;
    font-size: 1.1rem;
    font-family: 'Poppins', sans-serif;
    font-weight: 300;
}
</style>
