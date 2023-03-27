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

<ion-content id="main-content">
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
    <div>
      Matches:
      <ion-list v-for="match in matchData" :key="match">
        <ion-button @click="getPlayerData">
        {{ match.created.substring(0,match.created.length-5) }}
        </ion-button>
        <div v-if="showData">
        <div  v-for="name in names" :key="name">
          {{ name }}
          <div v-for="score in points[0]" :key="score">
            Bahn: {{ score.trackID}}
            Punkte: {{ score.score}}
          </div>
        </div>
      </div>
      </ion-list>
    </div>
  
</ion-content>



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
            
            userInfo: {},
            userMatches: [],
            matchData: [],
            names: [],
            points: [],
            showData: false
            
        };
    },
    async mounted(){
      console.log(db.authStore.isValid)
      if(db.authStore.isValid){
        this.userInfo = db.authStore.model
        this.userMatches = (await db.collection("users").getOne(db.authStore.model.id)).matches.matches;
        //console.log(this.userMatches)
        
        for (let i = 0; i < this.userMatches.length; i++) {
          this.matchData.push(await db.collection("games").getOne(this.userMatches[i]));
          //console.log(this.matchData[i].data.pointData[i].name)
        }
        console.log(this.matchData)
        

      }else{
        window.location.href = "/login";
      }
    },
    methods: {
        logout() {
            db.authStore.clear();
            location.reload();
        },
        getPlayerData() {
          this.showData = !this.showData
          console.log(this.show)
          for(let i = 0; i < this.matchData.length; i++){
            this.names.push(this.matchData[i].data.pointData[i].name)
            //console.log(this.names[i])
        }
          for(let j = 0; j <this.names.length; j++){
            this.points.push(this.matchData[j].data.pointData[j].score)
            
          }
          console.log(this.points)
          
    }
    },
    computed: {
      
    }
});
</script>
