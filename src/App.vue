<template>
  <ion-app>
    <ion-router-outlet />
  </ion-app>
</template>

<script lang="ts">
import { IonApp, IonRouterOutlet } from '@ionic/vue';
import { defineComponent } from 'vue';
import GameToolbar from './views/GameToolbar.vue';
import SideMenu from './views/SideMenu.vue';


export default defineComponent({
  name: 'App',
  components: {
    IonApp,
    IonRouterOutlet,
    
    
    
  }
});
</script>