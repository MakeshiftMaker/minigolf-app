<template>
    <ion-page>
        <ion-header>
        <ion-toolbar>
            <ion-title>Register</ion-title>
        </ion-toolbar>
        </ion-header>
        <ion-content>
        <ion-list>
            <ion-item>
            <ion-label position="floating">Email</ion-label>
            <ion-input type="email" v-model="email"></ion-input>
            </ion-item>
            <ion-item>
            <ion-label position="floating">Passwort</ion-label>
            <ion-input type="password" v-model="password"></ion-input>
            </ion-item>
            <ion-item>
            <ion-label position="floating">Passwort Wiederholen</ion-label>
            <ion-input type="password" v-model="password_check"></ion-input>
            </ion-item>
            <ion-item>
            <ion-label position="floating">Username</ion-label>
            <ion-input type="text" v-model="username"></ion-input>
            </ion-item>
        </ion-list>
        <ion-button @click="register">Registrieren</ion-button>
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
            email: '',
            password: '',
            password_check: '',
            username: '',
        };
    },
    methods: {
        
        async register() {
        
            try{
                if(this.email === "" || this.password === "" || this.password_check === "" || this.username === "") {
                    alert("Bitte alle Felder ausfüllen");
                } else
                if (this.password !== this.password_check) {
                    alert("Passwörter stimmen nicht überein");
                } else if(this.password.length < 8) {
                    alert("Passwort muss mindestens 8 Zeichen lang sein");
                } else {
                        const data = {
                    "username": this.username,
                    "email": this.email,
                    "emailVisibility": true,
                    "password": this.password,
                    "passwordConfirm": this.password_check,
                    "name": this.email
                };
                const record = await db.collection('users').create(data);

                        alert("Registrierung erfolgreich");
                        window.location.href = "/home";
                    }
                }
             catch (err){
                alert("Registrierung fehlgeschlagen, ist die E-Mail Adresse bereits registriert?");
                console.log(err)
            }
        
        },
    }
});
</script>

<style>

</style>