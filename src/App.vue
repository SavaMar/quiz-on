<template>
  <!-- prettier-ignore -->
  <main class="bg-gray-200 h-screen">
    <NavBar />
    <!-- <quiz-questions/> -->
    <Transition name="slide-fade" :duration="50">
      <starting-page class="p-10" v-show="!showQuiz"> 
        <button @click="showQuizClick" class="bg-transparent mt-7 mb-4 btn-black font-semibol btn rounded">
          Try on trial
        </button>
      </starting-page> 
    </Transition>
    <Transition name="slide-fade" :duration="550">
      <quiz-questions v-show="showQuiz"/>
    </Transition> 
  </main>
</template>

<script setup>
import { ref } from "vue";
import QuizQuestions from "./components/QuizQuestions.vue";
import NavBar from "./components/NavBar.vue";
import StartingPage from "./components/StartingPage.vue";

const showQuiz = ref(false);

const showQuizClick = () => {
  showQuiz.value = true;
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
