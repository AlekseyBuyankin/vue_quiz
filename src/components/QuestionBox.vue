<template>
  <div class="question-box-container">
    <b-jumbotron>
      <template v-slot:lead>{{ current_question.question }}</template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item
          v-for="(answer, index) in shuffled_answers"
          :key="index"
          @click="selectAnswer(index)"
          :class="classAnswer(index)"
          >{{ answer }}</b-list-group-item
        >
      </b-list-group>

      <b-button variant="primary" @click="submitAnswer()" :disabled="isAnswered"
        >Submit</b-button
      >

      <b-button
        @click="
          next_question(isAnswered);
          next(isAnswered);
        "
        variant="success"
        >Next</b-button
      >
    </b-jumbotron>
  </div>
</template>

<script>
import _ from "lodash";

export default {
  props: {
    current_question: Object,
    next_question: Function,
    increment: Function,
  },
  data() {
    return {
      selected_index: null,
      shuffled_answers: [],
      correct_index: null,
      isCorrect: false,
      isAnswered: false,
    };
  },
  computed: {
    answers() {
      let answers = [...this.current_question.incorrect_answers];
      answers.push(this.current_question.correct_answer);
      return answers;
    },
  },
  watch: {
    current_question: {
      immediate: true,
      handler() {
        this.selected_index = null;
        this.isAnswered = false;
        this.shuffleAnswers();
      },
    },
  },
  methods: {
    selectAnswer(index) {
      this.selected_index = index;
    },
    next(isAnswered) {
      if (isAnswered) {
        this.selected_index = null;
      }
    },
    shuffleAnswers() {
      let answers = [
        ...this.current_question.incorrect_answers,
        this.current_question.correct_answer,
      ];

      this.shuffled_answers = _.shuffle(answers);

      this.correct_index = this.shuffled_answers.indexOf(
        this.current_question.correct_answer
      );

      console.log(this.shuffled_answers[this.correct_index]);
    },
    submitAnswer() {
      if (this.selected_index === this.correct_index) {
        this.isCorrect = true;
      } else {
        this.isCorrect = false;
      }

      this.increment(this.isCorrect);
      this.isAnswered = true;
    },
    classAnswer(index) {
      let class_answer = "";

      if (!this.isAnswered && this.selected_index === index) {
        class_answer = "selected";
      } else if (this.isAnswered && this.correct_index === index) {
        class_answer = "correct";
      } else if (
        this.isAnswered &&
        this.selected_index === index &&
        this.correct_index !== index
      ) {
        class_answer = "incorrect";
      }

      return class_answer;
    },
  },
};
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}

.btn {
  margin: 0 5px;
}
.btn:hover {
  background: #eee;
  cursor: pointer;
}

.selected {
  background-color: blueviolet;
}
.selected:hover {
  background-color: blueviolet;
}

.correct {
  background-color: aquamarine;
}
.correct:hover {
  background-color: aquamarine;
}

.incorrect {
  background-color: rgb(247, 94, 94);
}
.incorrect:hover {
  background-color: rgb(247, 94, 94);
}

.jumbotron {
  padding: 2rem 2rem;
}
</style>
