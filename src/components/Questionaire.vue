<template>
  <div>
    <div class="row">
      <div class="col-md-12">
        <b-nav-item disabled class="left">Questions Answered: {{ numCorrect }}/{{ numTotal }}</b-nav-item>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <div class="jumbotron text-center">
          <h2 class="card-title h2 font-weight-bold">{{ currentQuestion.question }}</h2>
          <hr class="my-4" />
          <!-- <h4 class="blue-text my-4 font-weight-bold">Options</h4> -->
          <ul class="list-group">
            <li
              class="list-group-item my-1"
              id="clickGroup"
              v-for="(answer, index) in shuffledAnswers"
              :key="index"
              @click="selectAnswer(index)"
              :class="answerClass(index)"
            >{{ answer }}</li>
          </ul>

          <div class="pt-2">
            <button
              type="button"
              id="myButton"
              @click="next"
              class="btn btn-primary"
              :disabled="!answered"
            >next</button>
            <button
              type="button"
              id="submitButton"
              class="btn btn-success"
              @click="submitAnswer"
              :disabled="selectedIndex === null"
              onclick="document.getElementById('submitButton').disabled=true;"
            >Submit</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
    numCorrect: Function,
    numTotal: Function,
  },
  data: function () {
    return {
      selectedIndex: null,
      correctIndex: null,
      shuffledAnswers: [],
      answered: false,
    };
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers;
    },
  },
  watch: {
    currentQuestion: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
    },
    submitAnswer() {
      let isCorrect = false;

      if (this.selectedIndex === this.correctIndex) {
        isCorrect = true;
      }
      this.answered = true;

      this.increment(isCorrect);
    },
    shuffleAnswers() {
      let answers = [
        ...this.currentQuestion.incorrect_answers,
        this.currentQuestion.correct_answer,
      ];
      this.shuffledAnswers = _.shuffle(answers);
      this.correctIndex = this.shuffledAnswers.indexOf(
        this.currentQuestion.correct_answer
      );
    },
    answerClass(index) {
      let answerClass = "";

      if (!this.answered && this.selectedIndex === index) {
        answerClass = "selected";
      } else if (this.answered && this.correctIndex === index) {
        answerClass = "correct";
      } else if (
        this.answered &&
        this.selectedIndex === index &&
        this.correctIndex !== index
      ) {
        answerClass = "incorrect";
      }

      return answerClass;
    },
  },
};
</script>

<style scoped>
.list-group-item {
  background-color: #e9ecef;
  border: none;
}
.list-group-item:hover {
  color: white;
  background-color: #007bff;
  border: #007bff;
  cursor: pointer;
}
li.nav-item.left {
  color: green !important;
  font-size: 20px;
  float: right;
}
.nav-link.disabled {
  color: white;
}
.selected {
  background-color: lightblue;
  border: lightblue;
  color: white;
}
.correct {
  background-color: green;
  border: green;
  color: white;
}
.incorrect {
  background-color: red;
}
</style>
