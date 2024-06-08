<template>
  <ion-page>
    <ion-content :fullscreen="true">

      <div class="center-image w-logo" style="margin-top: 15vh;">
        <img src="../assets/graphics/logo-biomedforyou.png"></img>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent } from '@ionic/vue';
import ExploreContainer from '@/components/ExploreContainer.vue';
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
</style>
