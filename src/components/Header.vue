<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>
        <b>Fancy Quiz App</b>
      </b-nav-item>
      <b-nav-item disabled>
        Question:
        {{ current_question_index + 1 }}/{{ questions_length + 1 }}
      </b-nav-item>

      <b-nav-item disabled
        >Counter: {{ num_correct }}/{{ questions_length + 1 }}</b-nav-item
      >
      <b-button @click="reset()" variant="reset" href="#">Reset</b-button>
    </b-nav>
  </div>
</template>

<script>
export default {
  props: {
    current_question_index: Number,
    questions_length: Number,
    num_correct: Number,
    reset: Function,
  },
};
</script>
