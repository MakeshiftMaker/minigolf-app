<template>
     <ion-toolbar>
    <ion-buttons slot="start">
      <ion-back-button @click="$router.go(-1)" default-href="#"></ion-back-button>
    </ion-buttons>
   
  </ion-toolbar>
</template>

<script lang="ts">
  import { IonBackButton, IonButton, IonButtons, IonIcon, IonMenuButton, IonTitle, IonToolbar } from '@ionic/vue';
  import { create, ellipsisHorizontal, ellipsisVertical, helpCircle, search, personCircle, star } from 'ionicons/icons';
  import { defineComponent } from 'vue';

  export default defineComponent({
    components: { IonBackButton, IonButtons,  IonToolbar },
    setup() {
      return { create, ellipsisHorizontal, ellipsisVertical, helpCircle, search, personCircle, star };
    },
  });
</script>