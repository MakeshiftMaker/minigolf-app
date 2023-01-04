<template>
    <ion-page>
        <ion-header>
        <ion-toolbar>
            <div class="logo-card">
                <ion-card>
                    <img alt="Silhouette of mountains" src="../img/favicon.svg" />
                </ion-card>
            </div>
        </ion-toolbar>
        </ion-header>
        <ion-content class="game-buttons">
            <ion-button href="/game">Neues Spiel</ion-button>
            <ion-button href="/login">Einloggen</ion-button>
            <ion-button href="/register">Registrieren</ion-button>
        </ion-content>
    </ion-page>
</template>

<script>
 import { IonCard, IonCardContent, IonCardHeader, IonCardSubtitle, IonCardTitle } from '@ionic/vue';
  import { defineComponent } from 'vue';

  export default defineComponent({
    components: { IonCard },
  });
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Quicksand&display=swap');

ion-card{
    border-radius: 50px;
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
    background: #B8D8C9;
    
}

.game-buttons {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-top: 20%;
    /* i fucking love when I tell css to to a very menial task and nothing happends*/
}

ion-button {
    --background: #FFFFFF !important;
    --background-hover: #8CC4A9; 
    --border-radius: 15px;
    border: 3px solid #314D3F;
    border-radius: 18px;
    width: 274px;
    height: 55px;
    font-weight: 700;
    font-size: 23px;
    line-height: 29px;

    display: flex;
    align-content: center;
}

.button {
    --background: #FFFFFF !important;
    --background-hover: #8CC4A9; 
    --border-radius: 15px;

    border: 3px solid #314D3F;
    border-radius: 18px;
    width: 274px;
    height: 55px;
    font-weight: 700;
    font-size: 23px;
    line-height: 29px;

    display: flex;
}

html {
    font-style: normal;
    font-weight: 700;
    font-size: 28px;
    line-height: 35px;
    margin: 3%
}

*{
    color: #314D3F;
    font-family: 'Quicksand';

    --ion-background-color: #E5E5E5 !important;
    --ion-toolbar-background: #E5E5E5;

    
}

/* dumme Linie im Header */
.header-md::after{
    display: none; 
}

/* Title Font  */
.title-default {
    font-weight: 700;
    font-size: 17px;
    line-height: 21px;
}

.label-floating {
    font-weight: 700;
    font-size: 28px;
    line-height: 35px;
    margin-left: 2% !important; 
}

.list-md {
    background: #E5E5E5
}

.in-list {
    --background: #B8D8C9; 
    border-radius: 15px; 
    max-width: 243px !important;
    max-height: 43px !important;
    box-shadow: 0px 4px 6px rgba(49, 77, 63, 0.5);
    
    margin-top: 3%; 
    margin-bottom: 3%; 
    margin-left: 1%; 
    
    font-weight: 600;
    font-size: 20px;
    line-height: 25px;
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