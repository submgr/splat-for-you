<template>
  <ion-page>
    <ion-content :fullscreen="true">

      <div class="image-container">
        <video :src="currentSliderVideo" autoplay loop muted playsinline class="fullscreen-video" />
        <ion-button expand="block" class="bottom-button" style="margin-bottom: 2vh;"
          @click="goNextOne">{{ currentSliderButtonText }}</ion-button>
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
        <ion-button expand="block" class="bottom-button" style="margin-bottom: 2vh;"
          @click="goNextOne">{{ currentSliderButtonText }}</ion-button>
      </div>



    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonButton } from '@ionic/vue';
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
    IonButton
  },
  data() {
    return {
      currentSliderVideo: null,
      currentSliderButtonText: null,
      currentSlideId: -1,
      screens: [
        {
          buttontext: 'Tab 1',
          image: "https://splat-for-you.vercel.app/assets/video_novella/1.webm"
        },
        {
          buttontext: 'Tab 2',
          image: "https://splat-for-you.vercel.app/assets/video_novella/2.webm"
        },
        {
          buttontext: 'Tab 3',
          image: "https://splat-for-you.vercel.app/assets/video_novella/3.webm"
        },
        {
          buttontext: 'Tab 4',
          image: "https://splat-for-you.vercel.app/assets/video_novella/4.webm"
        },
        {
          buttontext: 'Tab 5',
          image: "https://splat-for-you.vercel.app/assets/video_novella/5.webm"
        },
        {
          buttontext: 'Tab 6',
          image: "https://splat-for-you.vercel.app/assets/video_novella/6.webm"
        },
        {
          buttontext: 'Tab 7',
          image: "https://splat-for-you.vercel.app/assets/video_novella/7.webm"
        },
        {
          buttontext: 'Tab 8',
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
      this.goToSlide(this.currentSlideId + 1);
    },

  },
  mounted() {
    const tabsEl = document.querySelector('ion-tab-bar');
    if (tabsEl) {
      tabsEl.hidden = false;
      tabsEl.style.height = "1";
    }

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
  min-width: 100%;
  min-height: 100%;
  width: auto;
  height: auto;
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