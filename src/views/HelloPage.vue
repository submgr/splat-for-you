<template>
  <ion-page>
    <ion-content :fullscreen="true">

      <div class="center-image w-logo" style="margin-top: 25vh;">
        <img src="../assets/graphics/logo-biomedforyou.png"></img>
      </div>

      <div class="spinner-container" style="margin-top: 10vh; transform: scale(1.4);">
        <ion-spinner name="crescent"></ion-spinner>
      </div>

    </ion-content>
  </ion-page>
</template>


<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
import { onMounted } from 'vue';
import { set } from 'cypress/types/lodash';

import {
  defineComponent
} from 'vue';

export default defineComponent({
  name: 'Tab1Page',
  components: {
    IonPage, IonHeader, IonToolbar, IonTitle, IonContent
  },
  mounted() {
    // eslint-disable-next-line
    const parent_this = this;

    const tabsEl = document.querySelector('ion-tab-bar');
    console.log(tabsEl)
    if (tabsEl) {
      tabsEl.hidden = true;
      tabsEl.style.height = "1";
      tabsEl.style.display = 'none'
    }

    setTimeout(() => {
      tabsEl.hidden = false;
      tabsEl.style.height = "1";
      tabsEl.style.display = 'flex'
      this.$router.push({ path: '/tabs/tab1', replace: false });
    }, 5000)
  },
  setup() {
    return {
    }
  }
});
</script>

<style scoped>
.center-image {
  display: block;
  margin-left: auto;
  margin-right: auto;
}

.w-logo {
  width: 70%;
}

.spinner-container {
  display: flex;
  justify-content: center;
}
</style>
