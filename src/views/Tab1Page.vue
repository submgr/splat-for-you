<template>
    <ion-page>
        <ion-content :fullscreen="true">

            <div class="container">
                <div class="image-container">
                    <img :src="image1" alt="Image 1">
                </div>
                <div class="image-container">
                    <img :src="image2" alt="Image 2">
                </div>
            </div>




        </ion-content>
    </ion-page>
</template>

<script lang="ts">
import { IonPage, IonHeader, IonToolbar, IonTitle, IonContent, IonButton } from '@ionic/vue';
import { Camera, CameraResultType } from '@capacitor/camera';

import image1 from '../assets/graphics/love-conquers-all.png';
import image2 from '../assets/graphics/love-conquers-all.png';

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
            image1,
            image2
        }
    },
    methods: {
        
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
.container {
    display: flex;
    height: 100vh;
}

.image-container {
    flex: 1;
    background-size: cover;
    background-position: center;
}

img {
    width: 100%;
    height: 100%;
    object-fit: cover;
}
</style>