<template>
    

<ion-menu content-id="main-content">
  <ion-header>
    <ion-toolbar>
      <ion-title>Menu Content</ion-title>
    </ion-toolbar>
  </ion-header>
  <ion-content class="ion-padding">

    <ion-button href="/home">Hauptmenü</ion-button>
    <ion-button href="/game">Neues Spiel</ion-button>
    <ion-button href="/profile">Profil</ion-button>
    <ion-button>Optionen</ion-button>
    <ion-button>Über</ion-button>


  </ion-content>
</ion-menu>

<ion-page id="main-content">
  <ion-header>
    <ion-toolbar>
      <ion-buttons slot="start">
        <ion-menu-button></ion-menu-button>
      </ion-buttons>
      <ion-title>Profile</ion-title>
    </ion-toolbar>
  </ion-header> 
  <ion-card>
    <img style="border-radius: 30px" src="../img/Profile_avatar_placeholder_large.png" alt="">
  </ion-card>
   <ion-card-header>
      <ion-card-title>Benutzer: {{ this.userInfo.name }}</ion-card-title>
      <ion-card-title>email {{ this.userInfo.email }}</ion-card-title>
      <ion-button color="danger" @click="logout">Ausloggen</ion-button>
    </ion-card-header>

  <ion-list v-for="match in userMatches" :key="match">{{ match }}</ion-list>

</ion-page>



</template>

<script>
import db from "./database/db.js"
import {IonMenu, IonHeader, IonToolbar, IonTitle,IonContent, IonButton} from '@ionic/vue';
import { defineComponent } from 'vue';


export default defineComponent({
    name: 'RegisterPage',
    components: {
        IonMenu,
        IonHeader,
        IonToolbar,
        IonTitle,
        IonContent,
        IonButton
      },
    data() {
        return {
            loggedIn: false,
            userInfo: {},
            userMatches: [],
        };
    },
    async mounted(){
      console.log(db.authStore.isValid)
      if(db.authStore.isValid){
        this.loggedIn = true;
        console.log("Logged in");
        this.userInfo = db.authStore.model
        this.userMatches = (await db.collection("users").getOne(db.authStore.model.id)).matches.matches;
        console.log(this.userMatches)
        
        
        console.log(this.userMatches)

      }else{
        this.loggedIn = false;
        window.location.href = "/login";
      }
    },
    methods: {
        logout() {
            db.authStore.clear();
            this.loggedIn = false;
            location.reload();
        }
    },
    
});
</script>
