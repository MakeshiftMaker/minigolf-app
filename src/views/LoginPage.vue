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
     
        <ion-toolbar>
          <ion-buttons slot="start">
            <ion-menu-button></ion-menu-button>
          </ion-buttons>
          <ion-title>Login</ion-title>
        </ion-toolbar>


    
        <ion-header>
        <ion-toolbar>
            
        </ion-toolbar>
        </ion-header>
        <ion-content>
        <ion-list>
            <div>E-Mail</div>
            <ion-item>
                <!--<ion-label>E-Mail</ion-label>-->
            <ion-input placeholder="E-Mail" type="email" v-model="email"></ion-input>
            </ion-item>
            <div>Passwort</div>
            <ion-item>
            <!--<ion-label>Passwort</ion-label>-->
            <ion-input placeholder="Passwort" type="password" v-model="password"></ion-input>
            </ion-item>
        </ion-list>
        <ion-button @click="login">Einloggen</ion-button>
        </ion-content>
    </ion-page>
</template>

<script>
import db from "./database/db.js"
import { IonInput, IonItem, IonList } from '@ionic/vue';
import { defineComponent } from 'vue';

export default defineComponent({
    name: 'RegisterPage',
    components: {
        IonInput,
        IonItem,
        
        IonList,
        
    },
    data() {
        return {
            email: "",
            password: "",
        };
    },
    methods: {
        async login() {
            const authData = await db.collection('users').authWithPassword(
            this.email,
            this.password,
            );

            // after the above you can also access the auth data from the authStore
            console.log(db.authStore.isValid);
            console.log(db.authStore.token);
            console.log(db.authStore.model.id);

            if(db.authStore.isValid) {
              window.location.href = "/profile";
            }
        }
    },
    
})

</script>

<style>

</style>