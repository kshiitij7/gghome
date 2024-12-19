<template>
  <v-app>

    <HeaderMain/>
    <NavBarMain/>
    
    <router-view/>
  
  </v-app>
</template>

<script>

import HeaderMain from './layouts/HeaderMain.vue';
import NavBarMain from './layouts/NavBarMain.vue';

export default {
  name: 'App',
  components:{
    HeaderMain,
    NavBarMain,
  },

  data: () => ({

  }),
}
</script>
  
