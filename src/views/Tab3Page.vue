<template>
  <ion-page>
    <ion-content :fullscreen="true">

      <div style="z-index: -99  !important; display: flex; justify-content: center; align-items: center;">
        <ion-spinner name="crescent"
          style="margin-top: 40vh; z-index: -99  !important; display: relative; transform: scale(2);"></ion-spinner>
      </div>

      <div class="image-container" style=" display: block;">
        <video :src="currentSliderVideo" autoplay loop muted playsinline class="fullscreen-video"
          style="margin-top: -2vh; z-index: 99 !important;" />
        <ion-button expand="block" class="bottom-button" style="margin-bottom: 5vh; z-index: 100 !important;"
          @click="goNextOne">{{
            currentSliderButtonText }}</ion-button>
      </div>

      <div style="display: none;">
        <div style="position: relative; width: 100%; height: 0; padding-top: 177.7778%;
 padding-bottom: 0; box-shadow: 0 2px 8px 0 rgba(63,69,81,0.16); margin-top: 1.6em; margin-bottom: 0.9em; overflow: hidden;
 border-radius: 8px; will-change: transform;">
          <iframe loading="lazy"
            style="position: absolute; width: 100%; height: 100%; top: 0; left: 0; border: none; padding: 0;margin: 0;"
            src="https:&#x2F;&#x2F;www.canva.com&#x2F;design&#x2F;DAGHi4PtmTA&#x2F;ipodtxk87_JoWotqaeTh7A&#x2F;watch?embed"
            allowfullscreen="allowfullscreen" allow="fullscreen">
          </iframe>
        </div>
        <ion-button expand="block" class="bottom-button" style="margin-bottom: 5vh;" @click="goNextOne">{{
          currentSliderButtonText }}</ion-button>
      </div>



    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonButton, IonSpinner } from '@ionic/vue';
import { Camera, CameraResultType } from '@capacitor/camera';

import image1 from '../assets/graphics/love-conquers-all.png';

import {
  defineComponent
} from 'vue';

export default defineComponent({
  name: 'Tab1Page',
  components: {
    IonContent,
    IonPage,
    IonButton,
    IonSpinner
  },
  data() {
    return {
      currentSliderVideo: null,
      currentSliderButtonText: null,
      currentSlideId: -1,
      screens: [
        {
          buttontext: 'Вперед!',
          image: "https://splat-for-you.vercel.app/assets/video_novella/1.webm"
        },
        {
          buttontext: 'А какие компонены есть?',
          image: "https://splat-for-you.vercel.app/assets/video_novella/2.webm"
        },
        {
          buttontext: 'Почему пероксиды лучше?',
          image: "https://splat-for-you.vercel.app/assets/video_novella/3.webm"
        },
        {
          buttontext: 'Что влияет на цвет эмали?',
          image: "https://splat-for-you.vercel.app/assets/video_novella/4.webm"
        },
        {
          buttontext: 'Спасибо! Расскажи еще что-нибудь',
          image: "https://splat-for-you.vercel.app/assets/video_novella/5.webm"
        },
        {
          buttontext: 'Что может травмировать эмаль?',
          image: "https://splat-for-you.vercel.app/assets/video_novella/6.webm"
        },
        {
          buttontext: 'Что выбрать ухода за полостью рта?',
          image: "https://splat-for-you.vercel.app/assets/video_novella/7.webm"
        },
        {
          buttontext: 'Спасибо за информацию!',
          image: "https://splat-for-you.vercel.app/assets/video_novella/8.webm"
        }
      ]
    }
  },
  methods: {
    goToSlide(index) {
      this.currentSlideId = index;
      this.currentSliderVideo = this.screens[index].image;
      this.currentSliderButtonText = this.screens[index].buttontext;
    },
    goNextOne() {
      if (this.currentSlideId == 0) {
        const tabsEl = document.querySelector('ion-tab-bar');
        console.log(tabsEl)
        if (tabsEl) {
          tabsEl.hidden = true;
          tabsEl.style.height = "1";
          tabsEl.style.display = 'none'
        }
      }
      if (this.currentSlideId + 1 == this.screens.length) {
        const tabsEl = document.querySelector('ion-tab-bar');
        console.log(tabsEl)
        if (tabsEl) {
          tabsEl.hidden = false;
          tabsEl.style.display = 'flex'
        }
        this.$router.push({path:'/tabs/hello', replace: false });
      }
      this.goToSlide(this.currentSlideId + 1);
    },

  },
  mounted() {
    console.log("test")

    this.goToSlide(this.currentSlideId + 1);
  },
  setup() {
    return {
    }
  }
});
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body {
  height: 100%;
  overflow: hidden;
}

.image-container {
  height: 100%;
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.fullscreen-video {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  z-index: -100;
  background-size: cover;
}

.bottom-button {
  position: fixed;
  bottom: 0;
  margin-left: 10vw;
  margin-right: 10vw;
  width: 80vw;
}
</style>