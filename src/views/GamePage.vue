<template>
    <ion-page>
        <ion-header>
        <ion-toolbar>
            <ion-title>Game</ion-title>
        </ion-toolbar>
        </ion-header>
        <ion-content class="nameEntry" v-if="nameEntry">
            <ion-list>
                Names:
                <ion-item v-for="name in names" :key="name">
                    {{name}}
                </ion-item>
            </ion-list>
            <ion-label>New Player</ion-label>
            <ion-input v-on:keyup.enter="onEnter" type="text" v-model="name"></ion-input>
            <ion-button @click="startGame">Start Game</ion-button>
        </ion-content>
        <ion-content class="counter" v-if="counter">
            {{names[turn]}}'s turn'
            strokes: {{strokes}}
            track: {{track+1}}
            <ion-button @click="strokes++">+</ion-button>
            <ion-button @click="negateStroke">-</ion-button>
            <ion-button @click="nextPlayer">Next Turn</ion-button>  
            <ion-button @click="debug">Leaderboard</ion-button> 
        </ion-content>
        <ion-content class="leaderBoard" v-if="leaderBoard">
            
            <ion-grid>
                <ion-row size="auto">
                    <ion-col size="auto">
                        Name
                    </ion-col>
                    <ion-col size="auto" v-for="player in pointData" :key="player.name">
                        {{player.name}}
                    </ion-col>
                </ion-row>
                <ion-row size="auto" v-for="track in tracks" :key="track">
                    {{track}}:
                    <ion-col size="auto" v-for="player in pointData" :key="player.name">
                        {{ player.score[track-1].score }}
                    </ion-col>
                </ion-row>
                <ion-row>
                    <ion-col size="auto">
                        Total
                    </ion-col>
                    <ion-col size="auto" v-for="player in pointData" :key="player.name">
                        {{player.score.reduce((a, b) => a + b.score, 0)}}
                    </ion-col>
                </ion-row>
            </ion-grid>




            
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
            name: '',
            names: ["ben", "joe", "jim"],
            nameEntry: true,
            counter: false,
            leaderBoard: false,
            turn: 0,
            strokes: 0,
            track : 0,
            tracks: [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18],
            pointData: [],
            gameID: "",
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
                alert("Please enter at least one name");
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
        },
        async nextPlayer() {
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
        }
    },          
});
</script>

<style>

</style>