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
    answer: 3,
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
    answer: 1,
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

    <section class="quiz-wrapper" v-if="!quizCompleted">
      <div class="quiz-info">
        <span class="question"> {{ getCurrentQuestions.questions }} </span>
      </div>

      <div class="options">
        <label
          v-for="(option, index) in getCurrentQuestions.options"
          :key="index"
          :class="`option ${
            getCurrentQuestions.answer == index
              ? index == getCurrentQuestions.answer
                ? 'correct'
                : 'incorrect'
              : ''
          }${
            getCurrentQuestions.selected != null &&
            index != getCurrentQuestions.selected
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

    <section v-else>
      <h2>You have completed this assessment</h2>
      <h3>Your Score: {{ Math.round((score / questions.length) * 100) }} %</h3>
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

.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  height: 100vh;
}

h1 {
  font-size: 2rem;
  margin-bottom: 2rem;
}

.quiz-wrapper {
  background-color: #669933;
  padding: 1rem;
  width: 100%;
  max-width: 650px;
  border-radius: 0.5rem;
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question {
  color: #090909;
  font-size: 1.5rem;
}

.options {
  margin-bottom: 1rem;
}

.option {
  padding: 1rem;
  display: block;
  color: #333;
  background-color: #fff;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover {
  background-color: #e6e6e6;
}

.option.correct {
  background-color: #00cc00;
}

.option.incorrect {
  background-color: #ed1c24;
}

.option:last-of-type {
  margin-bottom: 0;
}

.option.disabled {
  opacity: 0.5;
}

.option.input {
  display: none;
}

button {
  appearance: none;
  outline: none;
  border: none;
  padding: 1rem 2rem;
  margin-left: 12rem;
  background-color: aqua;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.2rem;
  border-radius: 0.5rem;
}

button:disabled {
  opacity: 0.5;
}

h3 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}
</style>
