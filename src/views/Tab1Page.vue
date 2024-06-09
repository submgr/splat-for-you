<template>
    <ion-page>
      <ion-content :fullscreen="true">
        <ion-header collapse="condense"  style="background-color: none; --background: none;">
          <ion-toolbar style="background-color: none; --background: none;">
            <ion-title size="large" >Добро пожаловать</ion-title>
          </ion-toolbar>
        </ion-header>
        <router-link to="/tabs/tab4">
          <div class="card bg5 borderradius30 card-smallheight" style="margin-top: 7%; margin-right: 4vw;"><div class="card_text"></div></div>
        </router-link>
        <!--<div class="card bg1 borderradius30" style="margin-top: 6%"><div class="card_text">Пройти тест</div></div>-->
  
        <!-- https://www.stonewall.org.uk/media/lgbt-facts-and-figures -->
        <ion-grid style="margin-top: -5%;"> 
          <ion-row>
            <ion-col size="6">
              <router-link to="/tabs/tab2">
                <div class="card bg2 height10vh borderradius30 noborders" style="margin-top: 4%"><div class="card_text"></div></div>
              </router-link>
            </ion-col>
            <ion-col>
              <router-link to="/tabs/tab3">
                <div class="card bg3 height10vh borderradius30 noborders" style="margin-top: 4%;"><div class="card_text direction-bottom-top-text" style="width: 400vw; height: 28vh; margin-left: -3vw !important;"><p></p></div></div>
              </router-link>
            </ion-col>
          </ion-row>
        </ion-grid>
        <router-link to="/tabs/howtosort">
          <div class="card bg6 borderradius30 card-smallheight" style="margin-top: 2%"><div class="card_text"></div></div>
        </router-link>
        <router-link to="/tabs/recyclepoints">
          <div class="card bg4 borderradius30 card-smallheight" style="margin-top: -1.9%"><div class="card_text"></div></div>
        </router-link>
        
        
      </ion-content>
      <ion-modal @willDismiss="Modal_onWillDismiss" :is-open="message_modal_isOpen == 'shareapp'" trigger="open-modal" :initial-breakpoint="0.55" :breakpoints="[0, 0.55]" handle-behavior="cycle">
          <ion-content class="ion-padding">
              <div class="ion-margin-top">
                  <ion-label style="white-space: pre-wrap;">{$t('home_shareapp_modalmessage')}</ion-label>
                  <br/><br/>
                  <div style="display: grid; place-items: center;">
                    <!---<img style="height: 20vh;" src="../assets/graphics/app-download-qrcode.svg"/>--->
                  </div>
                  
                  <br/>
                  <ion-button color="dark" @click="shareapp_external" expand="block">{$t('home_shareapp_modalmessage_sharebtn')}}</ion-button>
              </div>
          </ion-content>
      </ion-modal>
    </ion-page>
  </template>
  
  <style scoped>
  ion-header{
    margin-top: 2.3rem;
  }
  .noborders{
    margin-left: 0 !important;
    margin-right: 0 !important;
    width: 100% !important;
  }
  .direction-bottom-top-text{
    writing-mode: tb-rl;
    text-orientation: sideways;
    transform: rotate(-180deg);
  }
  ion-grid{
    margin-left: 0.3rem !important;
    margin-right: 0.8rem !important;
  }
  .height10vh{
    height: 16rem !important;
    width: 100% !important;
  }
  .height20vh{
    height: 20rem !important;
  }
  .borderradius30{
    border-radius: 30px !important;
  }
  .bg1{
    background: linear-gradient(to bottom, #4322500a 10%, #432250af), url('https://images.unsplash.com/photo-1531688335250-6fad807d3374?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80') !important;
  }
  .bg2{
    background: linear-gradient(to bottom, #43225000 10%, #43225000), url('../assets/graphics/SC-1.png') !important;
  }
  .bg3{
    background: linear-gradient(to bottom, #674ce000 10%, #6387c900), url('../assets/graphics/SC-2.png') !important;
  }
  .bg4{
    background: linear-gradient(to bottom, #16452800 10%, #00000000), url('../assets/graphics/SC_46_1.png') !important;
  }
  .bg5{
    background: linear-gradient(to bottom, #16452800 10%, #00000000), url('../assets/graphics/SC-3.png') !important;
  }
  .bg6{
    background: linear-gradient(to bottom, #16452800 10%, #00000000), url('../assets/graphics/SC_48_7.png') !important;
  }
  .card{
    position: relative;
    border-radius: 18px;
    height: 22vh;
    width: auto;
    margin-top: 7%;
    margin-left: 3.2%;
    background-size: cover !important;
  }
  .card-smallheight{
    height: 22vh;
    margin-bottom: 1.4rem;
  }
  .specialcard{
    position: relative;
    
    height: 28vh;
    width: auto;
    margin-top: 7%;
    margin-left: 1rem !important;
    margin-right: 1.3rem !important;
    border-radius: 24.4px;
    background-color: #000000;
    background-size: cover !important;
  }
  .specialcard::before {
    content: "";
    position: absolute;
    inset: 0;
    border-radius: 23.4px; 
    padding: 2.4px;
    background:linear-gradient(35deg,rgba(132, 229, 133, 0.96), rgba(11, 237, 56, 0.971)); 
    -webkit-mask: 
       linear-gradient(#fff 0 0) content-box, 
       linear-gradient(#fff 0 0);
    -webkit-mask-composite: xor;
            mask-composite: exclude; 
  }
  @media (prefers-color-scheme: light) {
      .specialcard::before {
        background:linear-gradient(35deg,rgba(132, 229, 133, 0.96), rgba(11, 237, 56, 0.971)); 
        padding: 2.9px;
      }
  }
  .card_text{
    bottom: 0 !important;
    margin-left: 5%;
    margin-right: 4%;
    text-align: left;
    position: absolute;
    white-space: normal; 
    word-wrap: break-word !important;
    background-image: linear-gradient(45deg, #cacac7 0%, #ffffff 50%, #fbfcdb 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 1.3rem;
    left: 0% !important;
    font-size: 29px;
    font-weight: 700;
  }
  .specialcard_text{
    bottom: 0 !important;
    margin-left: 5%;
    margin-right: 4%;
    text-align: left;
    position: absolute;
    white-space: normal; 
    word-wrap: break-word !important;
    background-image: linear-gradient(45deg, rgba(255, 255, 255, 0.544) 0%, #ffffffc0 50%, #feffdfd1 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    margin-bottom: 1.3rem;
    left: 0% !important;
    font-size: 34px !important;
    font-weight: 700;
  }
  </style>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonCol, IonGrid, IonRow, IonModal, IonButton, IonLabel } from '@ionic/vue';
  //import * as YandexAds from 'cordova-plugin-yandex-ads/www/yandexads';
  export default  defineComponent({
    name: 'Tab1Page',
    components: { IonHeader, IonToolbar, IonTitle, IonContent, IonPage, IonCol, IonGrid, IonRow, IonModal, IonButton, IonLabel },
    data(){
      return{
        message_modal_isOpen: "not"
      }
    },
    mounted(){
      try {
        //YandexAds.loadBanner();
        //window.addEventListener("bannerDidLoad", function(){
        //  YandexAds.showBanner();
        //});
      } catch (error) {
        console.error(error);
        // expected output: ReferenceError: nonExistentFunction is not defined
        // Note - error messages will vary depending on browser
      }
    },
    updated () {
      const tabsEl = document.querySelector('ion-tab-bar');
      if (tabsEl) {
        tabsEl.hidden = false;
        tabsEl.style.height = "1";
      }
    },
    methods: {
      shareapp_init(){
        this.message_modal_isOpen = "shareapp"
      },
      async shareapp_external(){
        // To be continued :) g
      },
      Modal_onWillDismiss(){
        this.message_modal_isOpen = "not";
      }
    }
  });
  </script>