<template>
    <ion-page>
        <ion-content :fullscreen="true" @click="handleClick">
            <flow-form ref="flowform" v-on:complete="onComplete" v-bind:language="language" v-bind:progressbar="false"
                v-bind:standalone="true">
                <question v-for="(question, index) in questions" v-bind="question" v-bind:key="'m' + index"
                    v-model="question.model">
                </question>

                <!-- Custom content for the Complete/Submit screen slots in the FlowForm component -->
                <template v-slot:complete>
                    <div class="f-section-wrap">
                        <div v-if="1 < 2">
                            <div v-if="loading" style="margin-left: -10vw;">
                                <AdvancedLoader origin="support-chat-is-loading"
                                    style="margin-top: 35vh; margin-left: -4.5vw;" />
                                <div class="center-image w-logo" style="margin-top: 05vh;">
                                    <img src="../assets/graphics/logo-leftaligned_full.png"></img>
                                </div>

                            </div>
                            <div v-else>
                                <div>

                                    <img :src="recommendation.image" style="height: 48vh; margin-bottom: -10vh; margin-top: -5vh; margin-left: -5vw; transform: rotate(80deg);">

                                    <ion-card
                                        style="margin-left: -2vw; margin-right: 2vw; margin-top: -7vh; border-radius: 20px;">
                                        <ion-card-header>
                                            <ion-card-title style="margin-top: -2vh;">{{ recommendation.title
                                                }}</ion-card-title>
                                            <ion-card-subtitle>
                                                <div class="center-image w-logo"
                                                    style="margin-left: 2px; margin-bottom: 0px; margin-top: 0vh;">
                                                    <img src="../assets/graphics/logo-blacktext.png"
                                                        style="height: 4vh;"></img>
                                                </div>
                                            </ion-card-subtitle>
                                        </ion-card-header>

                                        <ion-card-content>
                                            {{
                                                recommendation.description }}
                                        </ion-card-content>
                                    </ion-card>

                                    <ion-button @click="learnmodeaboutproduct" style="margin-left: -2vw; margin-right: 2vw; margin-top: 0vh; border-radius: 20px; text-align: left; " class="bottom-button"><span style="margin-left: 20px; margin-right: 30px;"> Узнать больше <span style="margin-left: 10px;"></span>🔎</span></ion-button>
                                </div>


                            </div>
                        </div>
                    </div>
                </template>
                <!-- We've overriden the default "complete" slot content -->
                <template v-slot:completeButton>
                    <div class="f-submit">
                        <!-- Leave empty to hide default submit button -->
                    </div>
                </template>
            </flow-form>
        </ion-content>
    </ion-page>
</template>

<style scoped>
rion-content {
    background: linear-gradient(to bottom, rgba(0, 0, 0, 0.121) 10%, rgba(2, 156, 12, 0.714)), url('https://images.unsplash.com/photo-1586078074298-05dca4848695?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1935&q=80') !important;
    background-position: 100%;
    background-size: cover !important;
}

.vff ul.f-radios li {
    border-radius: 20px !important;
}
</style>

<script lang="ts">
import { ref, onMounted } from 'vue'
import { defineComponent } from 'vue';
import { IonPage, IonContent, IonCard, IonCardContent, IonCardHeader, IonCardSubtitle } from '@ionic/vue';
import { FlowForm, Question, QuestionModel, QuestionType, ChoiceOption, LanguageModel } from '@ditdot-dev/vue-flow-form'

import confetti from 'canvas-confetti';

import AdvancedLoader from '../components/AdvancedLoader.vue';

export default defineComponent({
    name: 'Tab1Page',
    components: { IonContent, IonPage, FlowForm, Question, AdvancedLoader, IonCard, IonCardContent, IonCardHeader, IonCardSubtitle },
    data() {
        return {
            finalScore: 0,
            scores: [0, 0, 0, 0, 0, 0],
            loading: false,
            completed: false,
            recommendedProduct: null,
            firstClick: true,
            recommendation: {
                title: null,
                description: null,
                image: null,
                order_link: null
            },
            language: new LanguageModel({
                pressEnter: "",
                continue: "Вперед!"
            }),
            productsData: {
                sensitive: {
                    title: "Биомед Сенситив",
                    description: "Специальная зубная паста для чувствительных зубов и десен",
                    image: "https://i.ibb.co/6J9xwjS/Sensitive.png",
                    order_link: "https://biomed.ru/catalog/zubnye-pasty/superwhite/"
                },
                whitecomplex: {
                    title: "Биомед White Complex",
                    description: "Зубная паста для отбеливания зубов",
                    image: "https://i.ibb.co/bN8GgM7/White-Complex-B-1.png",
                    order_link: "https://biomed.ru/catalog/zubnye-pasty/superwhite/"
                },
                superwhite: {
                    title: "Биомед Superwhite",
                    description: "Зубная паста для максимального отбеливания зубов",
                    image: "https://i.ibb.co/0Cw37m7/Super-White.png",
                    order_link: "https://biomed.ru/catalog/zubnye-pasty/superwhite/"
                },
                calcimax: {
                    title: "Биомед Calcimax",
                    description: "Зубная паста для укрепления зубов и десен",
                    image: "https://i.ibb.co/FDNfYLp/Calcimax2.png",
                    order_link: "https://biomed.ru/catalog/zubnye-pasty/superwhite/"
                },
                vitafresh: {
                    title: "Биомед Vitafresh",
                    description: "Зубная паста для свежего дыхания",
                    image: "https://i.ibb.co/TbcvZ78/Vita-Fresh.png",
                    order_link: "https://biomed.ru/catalog/zubnye-pasty/superwhite/"
                },
                gumhealth: {
                    title: "Биомед Gum Health",
                    description: "Зубная паста для здоровья десен",
                    image: "https://i.ibb.co/HCGby04/Healthy-Gums.png",
                    order_link: "https://biomed.ru/catalog/zubnye-pasty/superwhite/"
                }

            },
            questions: [
                {
                    "type": 'sectionbreak',
                    "id": 'review',
                    "description": "Впереди вас ждет несколько вопросов, после которых вы узнаете, какая зубная паста Biomed® подходит вам больше всего!",
                    "tagline": '',
                    "title": 'Время найти зубную пасту вашей мечты!',
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "model": '',
                },
                {
                    "type": "multiplechoice",
                    "id": "smoking",
                    "title": "Курите ли вы?",
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "options": [
                        {
                            "label": "Да",
                            "value": "yes"
                        },
                        {
                            "label": "Нет",
                            "value": "no"
                        }
                    ],
                    "model": ""
                },
                {
                    "type": "multiplechoice",
                    "id": "red_wine",
                    "title": "Пьете ли вы красное вино?",
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "options": [
                        {
                            "label": "Нет, не употребляю алкоголь",
                            "value": "no_alcohol"
                        },
                        {
                            "label": "Да",
                            "value": "yes"
                        },
                        {
                            "label": "Предпочитаю другие алкогольные напитки",
                            "value": "other_alcohol"
                        }
                    ],
                    "model": ""
                },
                {
                    "type": "multiplechoice",
                    "id": "bad_breath",
                    "title": "Пахнет ли у вас изо рта?",
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "options": [
                        {
                            "label": "До чистки зубов утром.",
                            "value": "morning_breath"
                        },
                        {
                            "label": "Беспокоит в течении дня",
                            "value": "daytime_breath"
                        }
                    ],
                    "model": ""
                },
                {
                    "type": "multiplechoice",
                    "id": "opening_with_teeth",
                    "title": "Часто ли вы открываете что-то зубами?",
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "options": [
                        {
                            "label": "Нет, никогда",
                            "value": "never"
                        },
                        {
                            "label": "Да, периодически",
                            "value": "sometimes"
                        }
                    ],
                    "model": ""
                },
                {
                    "type": "multiplechoice",
                    "id": "gum_condition",
                    "title": "Состояние десен?",
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "options": [
                        {
                            "label": "Нет болезненных ощущений",
                            "value": "no_pain"
                        },
                        {
                            "label": "Часто кровоточат, возникают болезненные ощущения",
                            "value": "painful_and_bleeding"
                        }
                    ],
                    "model": ""
                },
                {
                    "type": "multiplechoice",
                    "id": "sweet_snacks",
                    "title": "Чаще всего ваши перекусы сладкие?",
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "options": [
                        {
                            "label": "Да, это так",
                            "value": "yes"
                        },
                        {
                            "label": "Периодически",
                            "value": "sometimes"
                        },
                        {
                            "label": "Нет, никогда",
                            "value": "never"
                        }
                    ],
                    "model": ""
                },
                {
                    "type": "multiplechoice",
                    "id": "tooth_sensitivity",
                    "title": "Чувствительность зубов?",
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "options": [
                        {
                            "label": "Повышенная чувствительность.",
                            "value": "high_sensitivity"
                        },
                        {
                            "label": "Средняя чувствительность.",
                            "value": "medium_sensitivity"
                        },
                        {
                            "label": "Стандартная чувствительность.",
                            "value": "normal_sensitivity"
                        }
                    ],
                    "model": ""
                },
                {
                    "type": "multiplechoice",
                    "id": "frequent_cavities",
                    "title": "Часто ли вы сталкиваетесь с кариесом?",
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "options": [
                        {
                            "label": "Практически не сталкиваюсь.",
                            "value": "rarely"
                        },
                        {
                            "label": "Часто нуждаюсь в профессиональном лечении.",
                            "value": "frequent_cavities"
                        }
                    ],
                    "model": ""
                },
                {
                    "type": "multiplechoice",
                    "id": "inflammatory_processes",
                    "tagline": "Сталкиваетесь ли вы с воспалительными процессами на постоянной основе?",
                    "title": "Воспалительные процессы",
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "options": [
                        {
                            "label": "Нет, не сталкиваюсь",
                            "value": "no"
                        },
                        {
                            "label": "Да, сталкиваюсь часто",
                            "value": "yes_often"
                        },
                        {
                            "label": "Время от времени",
                            "value": "sometimes"
                        }
                    ],
                    "model": ""
                },
                {
                    "type": "multiplechoice",
                    "id": "citrus_fruits_enjoyment",
                    "tagline": "Любите ли вы цитрусовые?",
                    "title": "Цитрусовые",
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "options": [
                        {
                            "label": "Часто употребляю их в пищу",
                            "value": "often"
                        },
                        {
                            "label": "Нечасто употребляю их в пищу",
                            "value": "not_often"
                        },
                        {
                            "label": "Совсем не употребляю",
                            "value": "never"
                        }
                    ],
                    "model": ""
                },
                {
                    "type": "multiplechoice",
                    "id": "dry_mouth_frequency",
                    "tagline": "Часто ли вы ощущаете сухость во рту?",
                    "title": "Сухость во рту",
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "options": [
                        {
                            "label": "Часто",
                            "value": "often"
                        },
                        {
                            "label": "Нечасто",
                            "value": "not_often"
                        }
                    ],
                    "model": ""
                },
                {
                    "type": "multiplechoice",
                    "id": "gastrointestinal_conditions",
                    "tagline": "Есть ли у вас заболевания ЖКТ, эндокринные болезни, инфекции?",
                    "title": "Заболевания ЖКТ и эндокринные болезни",
                    "multiple": false,
                    "required": true,
                    "helpTextShow": false,
                    "options": [
                        {
                            "label": "Да",
                            "value": "yes"
                        },
                        {
                            "label": "Нет",
                            "value": "no"
                        }
                    ],
                    "model": ""
                }
            ]
        }
    },
    mounted() {
        //
    },
    methods: {
        learnmodeaboutproduct(){
            window.open("https://splatglobal.com/products/", '_blank');
        },
        /* eslint-disable-next-line no-unused-vars */
        onComplete(completed, questionList) {
            // This method is called whenever the "completed" status is changed.
            this.completed = completed
            // Set `submitted` to true so the form knows not to allow back/forward
            // navigation anymore.
            //!!! this.$refs.flowform.submitted = true
            this.onSendData()
        },
        handleClick(event) {
            console.log('Page was clicked', event);
            // Handle the click event here
            if(this.firstClick == true){
                const tabsEl = document.querySelector('ion-tab-bar');
            console.log(tabsEl)
            if (tabsEl) {
            tabsEl.hidden = true;
            tabsEl.style.height = "1";
            tabsEl.style.display = 'none'
            }
            this.firstClick = false;
            }
        },
        onSendData() {
            // eslint-disable-next-line
            const self = this
            const data = this.getData()
            this.loading = true

            // eslint-disable-next-line
            const parent_this = this;

            /*
              You can use Fetch API to send the data to your server, eg.:
              fetch(url, {
                method: 'POST',
                headers: {
                  'Content-Type': 'application/json'
                },
                body: JSON.stringify(data)
              })
            */
            setTimeout(() => {
                self.loading = false

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

                const tabsEl = document.querySelector('ion-tab-bar');
                console.log(tabsEl)
                if (tabsEl) {
                    tabsEl.hidden = false;
                    tabsEl.style.height = "1";
                    tabsEl.style.display = 'flex'
                }
            }, 4500)
        },
        getData() {
            var data = {
                questions: [],
                answers: []
            }

            if (data.answers[0] == "no") {
                this.finalScore += 1;
            }

            if (data.answers[1] == "no_alcohol" || data.answers[1] == "other_alcohol") {
                this.finalScore += 1;
            }

            if (data.answers[2] == "daytime_breath") {
                this.finalScore += 3;
            }

            // if(data.answers[3]) {
            //     this.finalScore += 1;
            // }

            if (data.answers[4] == "painful_and_bleeding") {
                this.finalScore += 1;
            }

            if (data.answers[5] == "never") {
                this.finalScore += 1;
            }

            if (data.answers[6] == "high_sensitivity") {
                this.finalScore += 2;
            } else {
                this.finalScore += 2;
            }

            if (data.answers[7] == "rarely") {
                this.finalScore += 4;
            }

            if (data.answers[8] == "yes_often" || data.answers[8] == "sometimes") {
                this.finalScore += 1;
            }

            if (data.answers[9] == "often") {
                this.finalScore += 4;
            }

            if (data.answers[10] == "often") {
                this.finalScore += 1;
            }

            if (data.answers[11] == "yes") {
                this.finalScore += 1;
            }

            if (this.finalScore < 4) {
                this.recommendedProduct = "sensitive"
            } else if (this.finalScore < 9) {
                this.recommendedProduct = "whitecomplex"
            } else if (this.finalScore < 12) {
                this.recommendedProduct = "superwhite"
            } else if (this.finalScore < 16) {
                this.recommendedProduct = "calcimax"
            } else if (this.finalScore < 19) {
                this.recommendedProduct = "vitafresh"
            } else if (this.finalScore < 21) {
                this.recommendedProduct = "gumhealth"
            }

            this.recommendation = this.productsData[this.recommendedProduct];

            return data
        },
        goNext(event) {
            console.log("Going home!", event)
            this.$router.push({ path: '/tabs/hello', replace: true });
        }
    }
});
</script>

<style>
/* Import Vue Flow Form base CSS */
@import '../assets/css/vue-flow-form.css';
/* Import one of the Vue Flow Form CSS themes (optional) */
/* @import '~@ditdot-dev/vue-flow-form/dist/vue-flow-form.theme-minimal.css'; */
/* @import '~@ditdot-dev/vue-flow-form/dist/vue-flow-form.theme-green.css'; */
@import '../assets/css/quiz-flow.css';
</style>