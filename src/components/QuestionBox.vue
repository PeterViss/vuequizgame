<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>the question</template>
      {{question.question}}
      <hr class="my-4" />

      <b-list-group v-for="(answer, index) in answers" :key="index" @click="selectAnswer(index)">
        <b-list-group-item button>{{answer}}</b-list-group-item>
      </b-list-group>

      <!-- <p v-for="(answer, index) in answers" :key="index">{{answer}}</p> -->

      <b-button variant="primary" v-on:click="submitAnswer">Submit</b-button>
      <b-button @click="next" variant="success" href="#">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    question: Object,
    next: Function
  },
  data() {
    return {
      selectedIndex: null,
      shuffledAnswers: []
    };
  },
  computed: {
    answers() {
      let answers = [...this.question.incorrect_answers];
      answers.push(this.question.correct_answer);
      return answers;
    }
  },
  watch: {
    question: {
      immediate: true,
      handler() {
        this.selectedIndex = null;
        this.shuffleAnswers();
      }
    }
  },
  methods: {
    selectAnswer(index) {
      this.selectedIndex = index;
      console.log(this.selectedIndex);
    },
    shuffleAnswers() {
      let answers = [
        ...this.question.incorrect_answers,
        this.question.correct_answer
      ];
      this.shuffledAnswers = _.shuffle(answers);
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