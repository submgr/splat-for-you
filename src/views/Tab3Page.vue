<template>
  <ion-page>
    <ion-content :fullscreen="true">

      <div class="image-container" style="display: none;">
        <img :src="currentSliderImage" alt="Presentation slide" class="fullscreen-image" />
        <ion-button expand="block" class="bottom-button" style="margin-bottom: 2vh;" @click="goNextOne">{{currentSliderButtonText}}</ion-button>
      </div>

      <div>
        <iframe src="https://www.canva.com/design/DAGHi4PtmTA/ipodtxk87_JoWotqaeTh7A/watch" frameborder="0" style="border:0; width:100%; height:100%;"></iframe>
      <ion-button expand="block" class="bottom-button" style="margin-bottom: 2vh;" @click="goNextOne">{{currentSliderButtonText}}</ion-button>
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
      currentSliderImage: null,
      currentSliderButtonText: null,
      currentSlideId: -1,
      screens: [
        {
          buttontext: 'Tab 1',
          image: "https://i.ibb.co/SmHcmd8/1.jpg"
        },
        {
          buttontext: 'Tab 2',
          image: "https://i.ibb.co/T2smTHp/2.jpg"
        },
        {
          buttontext: 'Tab 3',
          image: "https://i.ibb.co/f2H1dfd/3.jpg"
        }
      ]
    }
  },
  methods: {
    goToSlide(index) {
      this.currentSlideId = index;
      this.currentSliderImage = this.screens[index].image;
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

html, body {
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

.fullscreen-image {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.bottom-button {
  position: fixed;
  bottom: 0;
  margin-left: 10vw;
  margin-right: 10vw; 
  width: 80vw;
}
</style>