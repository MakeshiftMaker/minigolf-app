<template>
    <ion-page>
        <ion-header>
        <ion-toolbar>
            <ion-title>Login</ion-title>
        </ion-toolbar>
        </ion-header>
        <ion-content>
        <ion-list>
            <ion-item>
            <ion-label >Email</ion-label>
            <ion-input type="email" v-model="email"></ion-input>
            </ion-item>
            <ion-item>
            <ion-label position="floating">Passwort</ion-label>
            <ion-input type="password" v-model="password"></ion-input>
            </ion-item>
        </ion-list>
        <ion-button @click="login">Einloggen</ion-button>
        </ion-content>
    </ion-page>
</template>

<script>
import db from "./database/db.js"
import { IonInput, IonItem, IonLabel, IonList } from '@ionic/vue';
import { defineComponent } from 'vue';

export default defineComponent({
    name: 'RegisterPage',
    components: {
        IonInput,
        IonItem,
        IonLabel,
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
        }
    }
})

</script>

<style>

</style>