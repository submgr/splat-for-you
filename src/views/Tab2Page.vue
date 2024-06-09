<template>
  <ion-page>
    <ion-content :fullscreen="true">


      <div style="margin-top: 5vh; margin-left: 6vw;" v-if="loading != true && finished != true">
        <h1 class="gradient-text big-text">Самое время</h1>
        <h1 class="gradient-text normal-text" style="margin-top: -0vh;">примерить <span class="underlined">улыбку</span>
        </h1>
        <ion-button style="margin-top: 3vh; margin-left: -2px;" @click="makePhoto()">Улыбнуться!</ion-button>
        <img class="bottom-image" src="../assets/graphics/love-conquers-all.png" style="width: 150vw;"></img>
      </div>

      <div v-if="loading == true" style="display: flex; justify-content: center; margin-top: 40vh;">
        <ion-spinner name="crescent"></ion-spinner>
      </div>



      <div v-if="loading != true && finished == true" style="margin-left: 5vw; margin-right: 4vw; margin-top: 4vh; ">

        <ion-segment :value="toggleView" style="width: 90vw;">
          <ion-segment-button value="0" @click="setToggleValue('0')">
            <ion-label>Просто отбеливание</ion-label>
          </ion-segment-button>
          <ion-segment-button value="1" @click="setToggleValue('1')">
            <ion-label>Улучшение с AI 🤩</ion-label>
          </ion-segment-button>
        </ion-segment>

        <ImgComparisonSlider style="border-radius: 20px; margin-top: 3.4vh;">
          <!-- eslint-disable -->
          <img slot="first" style="width: 100%" :src="image_before" />
          <img slot="second" v-if="toggleView == '0'" style="width: 100%" :src="image_after" />
          <img slot="second" v-if="toggleView == '1'" style="width: 100%" :src="image_after2" />
          <!-- eslint-enable -->
        </ImgComparisonSlider>

        <ion-button expand="block" class="bottom-button" style="margin-bottom: 3vh;" @click="goNextOne">Показать {{changingText1}}</ion-button>
      </div>



    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonButton, IonLabel, IonSegment, IonSegmentButton, IonSpinner } from '@ionic/vue';
import { Camera, CameraResultType } from '@capacitor/camera';

import { ImgComparisonSlider } from '@img-comparison-slider/vue';

import confetti from 'canvas-confetti';

import {
  defineComponent
} from 'vue';
import { set } from 'cypress/types/lodash';

export default defineComponent({
  name: 'Tab1Page',
  components: {
    IonContent,
    IonPage,
    ImgComparisonSlider,
    IonButton,
    IonLabel,
    IonSegment,
    IonSegmentButton,
    IonSpinner
  },
  data() {
    return {
      loading: false,
      finished: false,
      image_before: null,
      image_after: null,
      image_after2: null,
      toggleView: "0",
      changingText1: " "
    }
  },
  methods: {
    async setToggleValue(value) {
      this.toggleView = value;
    },
    async shareImage(){

    },
    async makePhoto() {
      // eslint-disable-next-line
      const parent_this = this;

      const image = await Camera.getPhoto({
        quality: 90,
        allowEditing: true,
        resultType: CameraResultType.Uri
      });

      // image.webPath will contain a path that can be set as an image src.
      // You can access the original file using image.path, which can be
      // passed to the Filesystem API to read the raw data of the image,
      // if desired (or pass resultType: CameraResultType.Base64 to getPhoto)
      let blob = await fetch(image.webPath).then(r => r.blob());
      parent_this.image_before = image.webPath;

      let formData = new FormData();
      formData.append("file", blob);

      var oReq = new XMLHttpRequest();
      oReq.open("POST", "https://hsecreate-machivevisionservice.deqstudio.com/newrequest", true);

      this.loading = true;

      oReq.onload = function (oEvent) {
        // When the request is complete, your code goes here.
        if (oReq.status >= 200 && oReq.status < 300) {
          // This will log the response text to the console.
          console.log(oReq.responseText);
          const hash = JSON.parse(oReq.responseText).hash;
          const final_img_url = `https://hsecreate-machivevisionservice.deqstudio.com/result/${hash}/after.jpg`;
          parent_this.image_after = final_img_url;
          const final_img_url2 = `https://hsecreate-machivevisionservice.deqstudio.com/result2/${hash}/after.jpg`;
          parent_this.image_after2 = final_img_url2;
          parent_this.finished = true;
          // var win = window.open("", "_blank");
          // win.document.write("<html><body><img src='" + final_img_url + "'></body></html>");

          var duration = 4 * 1000;
          var animationEnd = Date.now() + duration;
          var defaults = { startVelocity: 20, spread: 260, ticks: 90, zIndex: 0 };

          parent_this.loading = false;

          function randomInRange(min, max) {
            return Math.random() * (max - min) + min;
          }

          var interval = setInterval(function () {
            var timeLeft = animationEnd - Date.now();

            if (timeLeft <= 0) {
              return clearInterval(interval);
            }

            var particleCount = 100 * (timeLeft / duration);
            // since particles fall down, start a bit higher than random
            confetti(Object.assign({}, defaults, { particleCount, origin: { x: randomInRange(0.1, 0.3), y: Math.random() - 0.1 } }));
            confetti(Object.assign({}, defaults, { particleCount, origin: { x: randomInRange(0.7, 0.9), y: Math.random() - 0.1 } }));
          }, 250);
        } else {
          console.log("Error: " + oReq.statusText);
          alert("Произошла ошибка на сервере. Попробуйте позже!")
        }
      };

      oReq.onerror = function (oEvent) {
        // This will log any errors to the console.
        console.log("Request failed");
        alert("Произошла ошибка на сервере. Попробуйте позже!")
      };

      oReq.send(formData);
    }
  },
  mounted() {
    const tabsEl = document.querySelector('ion-tab-bar');
    if (tabsEl) {
      tabsEl.hidden = false;
      tabsEl.style.height = "1";
    }

    const changingText1Phrases = ["маме", "папе", "друзьям", "себе", "всему миру", "подруге", "другу", "соседу", "коту", "собаке", "попугаю", "рыбке"];
    var cht_len = changingText1Phrases.length;
    var cht_curr = 0;
    setInterval(() => {
      this.changingText1 = changingText1Phrases[cht_curr];
      cht_curr = (cht_curr + 1) % cht_len;
    }, 1000);

    //this.makePhoto();
  },
  setup() {
    return {
    }
  }
});
</script>

<style scoped>
img {
  object-fit: cover;
}

.bottom-image {
  position: absolute;
  bottom: -5vw;
  height: 40vh;
  left: -25vw;
}

.gradient-text {
  background: -webkit-linear-gradient(45deg, #65BFEB, #C497F1);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.big-text {
  font-size: 2.5rem;
  font-weight: 700;
  margin-top: 10vh;
  margin-bottom: 0;
}

.normal-text {
  font-size: 1.7rem;
  font-weight: 700;
  margin-top: 0;
  margin-bottom: 0;
}

.underlined {
  text-decoration: underline !important;
}

.button-container {
  display: flex;
  justify-content: space-between;
}

.rounded-button {
  display: flex;
  align-items: center;
  justify-content: center;
  border-radius: 50%;
  padding: 10px;
  background-color: #4c4545;
}

.rounded-button ion-icon {
  margin-right: 5px;
}
</style>
