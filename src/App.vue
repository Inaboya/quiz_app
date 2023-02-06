<script setup>
import { ref, computed } from 'vue';

const questions = ref([
  {
    questions: 'What is React ?',
    answer: 1,
    options: [
      'A framework for building frontend applications',
      'A library for building frontend applications',
      'A framework for building backend applications',
      'A library for building backend applications',
    ],
    selected: null,
  },

  {
    questions: 'HTML stands for ?',
    answer: 0,
    options: [
      'Hyper Text Markup Language',
      'Hyperlinks and Text Markup Language',
      'Hyper Text Markdown Language',
      'None of the above',
    ],
    selected: null,
  },
  {
    questions: 'What is true about meta tags?',
    answer: 4,
    options: [
      'Meta tags are those tags which go inside the Head tag of the HTML page',
      'Meta tags are not for the interface they are important for the browser.',
      'Meta Tags are always in name or value pairs ',
      'Meta tags consist of character encoding, title, or even description.',
      'All of the above',
    ],
    selected: null,
  },

  {
    questions: 'What is load balancing?',
    answer: 1,
    options: [
      'Load balancing is not distribution of all the incoming network traffic all across the backend servers.',
      'Load balancing is distribution of all the outgoing network traffic all across the backend servers.',
      'none of the above',
      'Load balancing is distribution of all the incoming network traffic all across the backend servers.',
    ],
    selected: null,
  },

  {
    questions: 'What is the difference between a class and an object?',
    answer: 1,
    options: [
      'A class is a blueprint for an object.',
      'An object is an instance of a class.',
      'A class is a blueprint for an object and an object is an instance of a class.',
      'none of the above',
    ],
    selected: null,
  },

  {
    questions: 'What does NPM stand for?',
    answer: 2,
    options: [
      'Node Package Module',
      'Node Package Manager',
      'Node Package Management',
      'None of the above',
    ],
    selected: null,
  },
]);

const quizCompleted = ref(false);

const currentQuestion = ref(0);

const score = computed(() => {
  let value = 0;

  questions.value.map((el) => {
    if (el.selected == el.answer) {
      value++;
    }
  });

  return value;
});

const getCurrentQuestions = computed(() => {
  let question = questions.value[currentQuestion.value];

  question.index = currentQuestion.value;

  return question;
});

const setAnswer = (e) => {
  questions.value[currentQuestion.value].selected = e.target.value;
  e.target.value = '';
};

const nextQuestion = () => {
  if (currentQuestion.value < questions.value.length - 1) {
    currentQuestion.value++;
    return;
  } else {
    quizCompleted.value = true;
  }
};
</script>

<template>
  <main class="app">
    <h1>Test Your Frontend Skills</h1>

    <section class="quiz-wrapper">
      <div class="quiz-info">
        <span class="question"> {{ getCurrentQuestions.questions }} </span>
        <span class="score">Score: {{ score }}/{{ questions.length }}</span>
      </div>

      <div class="options">
        <label
          v-for="(option, index) in getCurrentQuestions.options"
          :key="index"
          :class="`option ${
            getCurrentQuestions.selected == index
              ? index == getCurrentQuestions.answer
                ? 'correct'
                : 'incorrect'
              : ''
          }${
            getCurrentQuestions.selected != null &&
            index != getCurrentQuestions.answer
              ? 'disabled'
              : ''
          }`"
        >
          <input
            type="radio"
            :name="getCurrentQuestions.index"
            :value="index"
            v-model="getCurrentQuestions.selected"
            :disabled="getCurrentQuestions.selected"
            @change="setAnswer"
          />

          <span>{{ option }}</span>
        </label>
      </div>

      <button @click="nextQuestion" :disabled="!getCurrentQuestions.selected">
        {{
          getCurrentQuestions.index == questions.length - 1
            ? 'Submit'
            : getCurrentQuestions.selected == null
            ? 'Select an option'
            : 'Next Question'
        }}
      </button>
    </section>
  </main>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto+Slab&display=swap');
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Roboto Slab', 'Montserrat', sans-serif;
}

body {
  background-color: #cecece;
}
</style>
