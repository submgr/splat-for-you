<template>
  <ion-page>
    <ion-content :fullscreen="true">


      <div style="margin-top: 5vh; margin-left: 6vw;">
        <h1 class="gradient-text big-text">Самое время</h1>
        <h1 class="gradient-text normal-text" style="margin-top: -0vh;">примерить <span class="underlined">улыбку</span></h1>
        <ion-button style="margin-top: 3vh; margin-left: -2px;" @click="makePhoto()">Улыбнуться!</ion-button>
        <img class="bottom-image" src="../assets/graphics/love-conquers-all.png" style="width: 150vw;"></img>
      </div>

      <ImgComparisonSlider v-if="finished == true">
        <!-- eslint-disable -->
        <img slot="first" style="width: 100%" :src="image_before" />
        <img slot="second" style="width: 100%" :src="image_after" />
        <!-- eslint-enable -->
      </ImgComparisonSlider>

      

    </ion-content>
  </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonButton } from '@ionic/vue';
import { Camera, CameraResultType } from '@capacitor/camera';

import { ImgComparisonSlider } from '@img-comparison-slider/vue';

import {
  defineComponent
} from 'vue';

export default defineComponent({
  name: 'Tab1Page',
  components: {
    IonContent,
    IonPage,
    ImgComparisonSlider,
    IonButton
  },
  data() {
    return {
      finished: false,
      image_before: null,
      image_after: null
    }
  },
  methods: {
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
      parent_this.image_before= image.webPath;

      let formData = new FormData();
      formData.append("file", blob);

      var oReq = new XMLHttpRequest();
      oReq.open("POST", "https://hsecreate-machivevisionservice.deqstudio.com/newrequest", true);

      oReq.onload = function (oEvent) {
        // When the request is complete, your code goes here.
        if (oReq.status >= 200 && oReq.status < 300) {
          // This will log the response text to the console.
          console.log(oReq.responseText);
          const hash = JSON.parse(oReq.responseText).hash;
          const final_img_url = `https://hsecreate-machivevisionservice.deqstudio.com/result/${hash}/after.jpg`;
          parent_this.image_after = final_img_url;
          parent_this.finished = true;
          // var win = window.open("", "_blank");
          // win.document.write("<html><body><img src='" + final_img_url + "'></body></html>");
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
.big-text{
  font-size: 2.5rem;
  font-weight: 700;
  margin-top: 10vh;
  margin-bottom: 0;
}
.normal-text{
  font-size: 1.7rem;
  font-weight: 700;
  margin-top: 0;
  margin-bottom: 0;
}
.underlined {
  text-decoration: underline !important;
}
</style>
