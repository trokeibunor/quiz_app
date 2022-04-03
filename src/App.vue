<template>
  <div>
    <score-board />
    <template v-if="this.question">
      <h1 v-html="this.question"></h1>
      <template v-for="(answer, index) in this.answers" :key="index">
        <input
          type="radio"
          name="options"
          :value="answer"
          v-model="this.chosen_answer"
          :disabled="this.answerSubmitted"
        />
        <label v-html="answer"></label><br />
      </template>
      <button
        v-if="!this.answerSubmitted"
        @click="this.submitAnswer"
        class="send"
        type="button"
      >
        Send
      </button>
    </template>

    <section class="result" v-if="this.answerSubmitted">
      <template v-if="this.chosen_answer == this.correctAnswer">
        <h4
          v-html="
            '&#9989; Congratulations, the answer ' +
            this.correctAnswer +
            ' is correct.'
          "
        ></h4>
      </template>
      <template v-else>
        <h4
          v-html="
            '&#10060; I\'m sorry, you picked the wrong answer. The correct is ' +
            this.correctAnswer
          "
        ></h4>
      </template>
      <button @click="this.getNewQuestion()" class="send" type="button">
        Next question
      </button>
    </section>
  </div>
</template>

<script>
import ScoreBoard from "@/components/ScoreBoard.vue";
export default {
  name: "App",
  components: {
    ScoreBoard,
  },
  data() {
    return {
      question: undefined,
      incorrectAnswers: undefined,
      correctAnswer: undefined,
      chosen_answer: undefined,
      answerSubmitted: false,
    };
  },
  methods: {
    submitAnswer() {
      if (!this.chosen_answer) {
        alert("please choose an option");
      } else {
        this.answerSubmitted = true;
        if (this.chosen_answer == this.correctAnswer) {
          console.log("That is correct");
        } else {
          console.log("That is incorrect");
        }
      }
    },

    getNewQuestion() {
      this.answerSubmitted = false;
      this.chosen_answer = undefined;
      this.question = undefined;
      this.axios
        .get("https://opentdb.com/api.php?amount=1")
        .then((response) => {
          this.question = response.data.results[0].question;
          this.correctAnswer = response.data.results[0].correct_answer;
          this.incorrectAnswers = response.data.results[0].incorrect_answers;
        });
    },
  },
  computed: {
    answers() {
      var answers = JSON.parse(JSON.stringify(this.incorrectAnswers));
      answers.splice(
        Math.round(Math.random() * answers.length),
        0,
        this.correctAnswer
      );
      return answers;
    },
  },
  // Lifecycle hook
  created() {
    this.getNewQuestion();
  },
};
// https://opentdb.com/api.php?amount=1
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 960px;

  input[type="radio"] {
    margin: 12px 4px;
  }
  button.send {
    margin-top: 12px;
    height: 40px;
    min-width: 120px;
    padding: 0 16px;
    color: #fff;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    cursor: pointer;
  }
}
</style>
