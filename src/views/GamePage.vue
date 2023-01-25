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
            <ion-title>Game</ion-title>
        </ion-toolbar>
      </ion-header>


        <div class="trackEntry" v-if="trackEntry">
            <ion-slides>
                <ion-slide class="main-course">
                    <ion-button @click="this.mainCourse=true;this.trackEntry=false;this.nameEntry=true">Hauptplatz</ion-button>
                </ion-slide>

                <ion-slide class="miniature-course">
                    <ion-button @click="this.mainCourse=false;this.trackEntry=false;this.nameEntry=true">Miniatur-Platz</ion-button>
                </ion-slide>
            </ion-slides>
        </div>

        
        <div class="nameEntry" v-if="nameEntry">
            <div>Spieler</div>
            <ion-list>
                
                <ion-item v-for="name in names" :key="name">
                   {{name}}
                </ion-item>
            </ion-list>
            <ion-label>Namen Eingeben</ion-label>
            <ion-input v-on:keyup.enter="onEnter" type="text" v-model="name"></ion-input>
            <ion-button @click="startGame">Start Game</ion-button>
        </div>


        <div class="counter" v-if="counter">
            {{names[turn]}}'s turn'
            strokes: {{strokes}}
            track: {{track+1}}
            <ion-card class="buttons">
                <ion-button class="action-button" @click="strokes++">+</ion-button>
                <ion-button class="action-button" @click="negateStroke">-</ion-button>
            </ion-card>
                <ion-button @click="nextPlayer">nächster Spieler</ion-button>  
                <ion-button @click="surenessModal=true">Spiel Beenden</ion-button> 
                <ion-modal v-if="surenessModal">
                
                    <div>Sind Sie sicher, dass Sie das Spiel beenden wollen?</div>
                    <ion-button @click="endGame">Ja</ion-button>
                    <ion-button @click="surenessModal=false">Nein</ion-button>

                </ion-modal>
            </div>
            
         
        
    </ion-page>
</template>

<script>
import db from "./database/db.js"
import { IonInput, IonItem, IonLabel, IonList } from '@ionic/vue';
import { defineComponent } from 'vue';


export default defineComponent({
    name: 'GamePage',
    components: {
        IonInput,
        IonItem,
        IonLabel,
        IonList,
        
        
    },
    data() {
        return {
            name: '',
            names: ["Max", "Moritz", "Hans"],
            trackEntry: true,
            nameEntry: false,
            counter: false,
            leaderBoard: false,
            turn: 0,
            strokes: 0,
            track : 0, //current track for the player
            mainCourse: true, //main/miniature track
            tracks: [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18],
            pointData: [],
            gameID: "",
            surenessModal: false,
        };
    },
    methods: {
        
        async onEnter() {
            //only push names if they are not empty
            if(this.name != ''){
            this.names.push(this.name);
            this.name = '';
        }
            
        },
        async startGame() {
            if(this.names.length == 0){
                alert("Bitte mindestens einen Namen eingeben");
                return;
            }
            this.nameEntry = false;
            this.counter = true;
            
            for(let i = 0;i<this.names.length;i++){
                var name = this.names[i];
                var obj = {
                    "name": name,
                    "score": [{"trackID": 1, "score": 0}, {"trackID": 2, "score": 0}, {"trackID": 3, "score": 0}, {"trackID": 4, "score": 0}, {"trackID": 5, "score": 0}, {"trackID": 6, "score": 0}, {"trackID": 7, "score": 0}, {"trackID": 8, "score": 0}, {"trackID": 9, "score": 0}, {"trackID": 10, "score": 0}, {"trackID": 11, "score": 0}, {"trackID": 12, "score": 0}, {"trackID": 13, "score": 0}, {"trackID": 14, "score": 0}, {"trackID": 15, "score": 0}, {"trackID": 16, "score": 0}, {"trackID": 17, "score": 0}, {"trackID": 18, "score": 0}]
                }
                this.pointData.push(obj);

                
            }
            //console.log(test);
            
            this.gameID = Array.from(Array(15), () => Math.floor(Math.random() * 36).toString(36)).join('');
            //console.log(this.gameID);
            const data = {
                "data": {
                    "pointData": this.pointData,
                    
                },
                "id" : this.gameID,
                
            }
            const record = await db.collection("games").create(data);


            //add the ID to the Users mathces list

            const previousMatches = (await db.collection("users").getOne(db.authStore.model.id)).matches.matches;
            previousMatches.push(this.gameID);
            console.log(previousMatches)

            const userData = {
                "matches": {
                    "matches": previousMatches,
                    
                },
            }
            const userRecord = await db.collection("users").update(db.authStore.model.id, userData);
        },
        async nextPlayer() {
            console.log("next player");
            if(this.turn < this.names.length-1){
                //update the current players score in the database with the score they just got, then go to the next player
                //console.log(this.pointData);
                this.pointData[this.turn].score[this.track].score = this.strokes;
                const data = {
                "data": {
                    "pointData": this.pointData,
                    
                },
            }
                const record = await db.collection("games").update(this.gameID, data)

                this.turn++;
            }else{
                this.pointData[this.turn].score[this.track].score = this.strokes;
                const data = {
                "data": {
                    "pointData": this.pointData,
                    
                },
            }
                const record = await db.collection("games").update(this.gameID, data)
                this.turn = 0;
                this.track++;
            }
            this.strokes = 0;
            if(this.track >= 17){
                this.counter = false;
                this.leaderBoard = true;
            }
        },
        async negateStroke() {
            if(this.strokes > 0){
                this.strokes--;
            }
        },
        async debug(){
            this.nameEntry=false;
            this.counter=false;
            this.leaderBoard=true;
        },
        async endGame(){
            this.surenessModal = false;

            
            this.pointData[this.turn].score[this.track].score = this.strokes;
                const data = {
                "data": {
                    "pointData": this.pointData,
                    
                },
            }
                const record = await db.collection("games").update(this.gameID, data)

            window.location.href = "/home";
            this.sureness=false;
            
        },
    },          
});
</script>

<style>
/*Background image not working pls help*/
.main-course {
    background-image: "../img/minigolfplatz-bischofshofen-overwiew.gif";
}
.miniature-course {
    background-image: "../img/minigolfplatz-bischofshofen-overwiew.gif";
}
/*Die + und - buttons sollten höher aber dünner sein, und die ion-card ist für irgendeinen grund 6.000 meter hoch*/ 


.action-button{
    width: 80px;
    height: 140px;
    --background: #B8D8C9 !important;
    border: 0px; 
    font-size: 60px;
    margin: 10%;
}

.native-input {
    background-color: #B8D8C9 !important; 
    border-radius: 15px !important; 
    max-width: 243px !important;
    max-height: 43px !important;
    box-shadow: 0px 4px 6px rgba(49, 77, 63, 0.5);
    
    margin-top: 3%; 
    margin-bottom: 3%; 
    margin-left: 1%; 
    
    font-weight: 500 !important;
    font-size: 20px !important;
    line-height: 25px;
    color: #314D3F !important;
}  

</style>