<template>
  <div id="app">
    <Header
      v-if="questions.length"
      :current_question_index="index"
      :questions_length="questions.length"
      :num_correct="num_correct"
      :reset="reset"
    />

    <b-container class="bv-example-row">
      <b-row>
        <b-col sm="12" offset="0">
          <QuestionBox
            v-if="questions.length"
            :current_question="questions[index]"
            :next_question="next_question"
            :increment="increment"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from "./components/Header.vue";
import QuestionBox from "./components/QuestionBox.vue";

export default {
  name: "App",
  components: {
    Header,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index: 0,
      num_correct: 0,
    };
  },
  methods: {
    next_question(isAnswered) {
      if (isAnswered) {
        this.index++;
      }
    },
    increment(isCorrect) {
      if (isCorrect) {
        this.num_correct++;
      }
    },
    reset() {
      this.index = 0;
      this.num_correct = 0;
    },
  },
  mounted() {
    fetch(
      "https://opentdb.com/api.php?amount=10&category=22&difficulty=easy&type=multiple",
      {
        method: "get",
      }
    )
      .then((response) => {
        return response.json();
      })
      .then((jsonData) => {
        this.questions = jsonData.results;
      });
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
