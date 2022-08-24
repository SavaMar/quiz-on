<template>
  <!-- prettier-ignore -->

  <div>
    <Transition name="slide-fade" :duration="50" v-show="!quizCompleated">
      <div :class="{fadeIn: disabled}" class="bg-gray-700 h-screen text-white pt-20">
        <p class="mb-10">{{ getCurrentQuestion }}</p>
        <button class="btn btn-black mr-5" @click="goToNextQuestion(0)"> {{getFirstAnswer}}</button>
        <button class="btn btn-black" @click="goToNextQuestion(1)"> {{getSecondAnswer}}</button>
      </div>
    </Transition>

    <!-- Loader block -->
     <Transition name="slide-fade" :duration="550" v-show="loaderView">
        <div class="h-screen w-screen top-0 bg-gray-700 absolute max-w-screen-lg z-90 flex justify-center items-center overflow-hidden" >
          <h2 class="text-white relative bottom-20">
            We're running to get your result
          </h2>
          <div class="atom-spinner absolute">
            <div class="spinner-inner">
              <div class="spinner-line"></div>
              <div class="spinner-line"></div>
              <div class="spinner-line"></div>
              <div class="spinner-circle">
                &#9679;
              </div>
            </div>
          </div>
        </div>
    </Transition>

    <!-- End result block -->
    <Transition name="slide-fade" :duration="2550" v-show="quizCompleated">
      <div class="mt-20">
        <div class="text-left">
          <h1 class="text-2xl ml-12">Congratulations!</h1>
        </div>
        <div v-for="(value, key, index) in shoesSortedResult" v-bind:key="index">
          <div class="max-w-sm rounded overflow-hidden shadow-lg mt-10 mx-10  bg-gray-300 py-10 px-5" :class="{winner: index===0}">
            <img class="" :src="getImgUrl(key)" alt="Sunset in the mountains">
            <div class="px-6 py-4 text-left">
              <div class="font-bold text-xl mb-2 mt-2">{{key}}</div>
              <p class="text-gray-500 text-sm">Your perfect partner in the world's lightest fully-cushioned shoe for Running Remixed</p>
              <p class="text-gray-700 text-sm font-medium mt-2">200 CHF | Neon Grey</p>
            </div>
            <div class="px-4 pt-4 items-center">
              <button class="btn btn-black">Shop now</button>
            </div>
          </div>
            <div class="text-left" v-if="index === 0">
              <h1 class="text-2xl mt-8 ml-12">Similar profiles</h1>
            </div>
        </div>
      </div>
    </Transition>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import quizData from "../assets/data.json";

const shoes = ref(quizData.shoes);
const questions = ref(quizData.questions);

const questionIndex = ref(0);
const quizCompleated = ref(false);
const shoesResult = ref({});
const disabled = ref(false);
const loaderView = ref(false);

const getCurrentQuestion = computed(() => {
  let question = questions.value[questionIndex.value].copy;
  return question;
});

const getFirstAnswer = computed(() => {
  let firstAnswer = questions.value[questionIndex.value].answers[0].copy;
  if (!nextQuestionValid(0)) {
    return;
  }
  return firstAnswer;
});
const getSecondAnswer = computed(() => {
  let secondAnswer = questions.value[questionIndex.value].answers[1].copy;
  if (!nextQuestionValid(1)) {
    return;
  }
  return secondAnswer;
});

function nextQuestionValid(answerIndex) {
  let nextQuestion =
    questions.value[questionIndex.value].answers[answerIndex].nextQuestion;
  if (nextQuestion == "") {
    loaderView.value = true;
    setTimeout(function () {
      loaderView.value = false;
      quizCompleated.value = true;
    }, 3500);
    return false;
  }
  return true;
}

function goToNextQuestion(answerIndex) {
  disabled.value = true;
  setTimeout(function () {
    disabled.value = false;
  }, 500);
  let increaseValue = Object.create(
    questions.value[questionIndex.value].answers[answerIndex].ratingIncrease
  );
  ratingIncrease(increaseValue);
  questionIndex.value++;
}

const shoesSortedResult = computed(() => {
  let sortable = Object.entries(shoesResult.value)
    .sort(([, a], [, b]) => b - a)
    .reduce((r, [k, v]) => ({ ...r, [k]: v }), {});
  return sortable;
});

function ratingIncrease(increaseValue) {
  for (var i = 0; i < shoes.value.length; i++) {
    let shoesId = shoes.value[i].id;
    let shoesName = shoes.value[i].name;
    let shoesRatingNew = Number(increaseValue[shoesId]);

    shoes.value[i].rating += shoesRatingNew;
    shoesResult.value[shoesName] = Number(shoes.value[i].rating);
  }
}

function getImgUrl(pet) {
  let images = require.context("../assets/", false, /\.png$/);
  return images("./" + pet + ".png");
}
</script>

<style>
.winner {
  border: 3px solid #b83280;
}
</style>
