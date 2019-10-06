<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>the question</template>
      {{question.question}}
      <hr class="my-4" />

      <b-list-group
        v-for="(answer, index) in answers"
        :key="index"
        @click.prevent="selectAnswer(answer)"
        :class="[
         answeredClass(answer)]"
      >
        <b-list-group-item button>{{answer}}</b-list-group-item>
      </b-list-group>

      <b-button
        variant="primary"
        v-on:click="submitAnswer"
        :disabled="selectedAnswer === null || answered"
      >Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    question: Object,
    next: Function,
    increment: Function
  },
  data() {
    return {
      selectedAnswer: null,
      correctAnswer: null,
      shuffledAnswers: [],
      answered: false
    };
  },
  computed: {
    answers() {
      let answers = [...this.question.incorrect_answers];
      answers.push(this.question.correct_answer);
      console.log(answers);
      return answers;
    }
  },
  watch: {
    question: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.answered = false;
        this.shuffleAnswers();
      }
    }
  },
  methods: {
    selectAnswer(answer) {
      this.selectedAnswer = answer;
    },
    submitAnswer() {
      let isCorrect = false;

      if (this.selectedAnswer === this.correctAnswer) {
        isCorrect = true;
      }
      this.answered = true;
      this.increment(isCorrect);
    },
    shuffleAnswers() {
      let answers = [
        ...this.question.incorrect_answers,
        this.question.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);

      this.correctAnswer = this.question.correct_answer;
    },
    answeredClass(index) {
      let answerClass = "";
      if (this.answered && this.correctAnswer === index) {
        answerClass = "correct";
      } else if (
        this.answered &&
        this.selectedAnswer === index &&
        this.correctAnswer !== index
      ) {
        answerClass = "incorrect";
      }

      return answerClass;
    }
  }
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.btn {
  margin: 0 5px;
}
.correct {
  background-color: green;
}

.incorrect {
  background-color: red;
}
</style>