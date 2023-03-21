<template>

  <div>

    <ScoreBoard :winCount="this.winCount" :loseCount="this.loseCount" />

    <template v-if="this.question">
        <h1 v-html="this.question"></h1>

      <template v-for="(answar, index) in this.answars" :key="index">

        <input 
          :id="index" 
          :value="answar"
          :disabled="this.answarSubimitted" 
          type="radio" 
          name="options" 
          v-model="this.chosenAnswer"
        >
        <label :for="index" v-html="answar"></label><br>

      </template>

      <section v-if="this.answarSubimitted">
        <h4 v-if="this.correctAnswar == this.chosenAnswer">
          &#9989; você acertou! Parabéns
        </h4>

        <h4 
          v-else
          v-html="'&#10060; Opa! você errou... A resposta correta é ' + this.correctAnswar"
        >
        </h4>

        <button @click="this.getNewQuestion()" class="send" type="button">Proxima questão</button>
      </section>

      <button v-if="!this.answarSubimitted" @click="submitAnswer()" class="send" type="button">enviar</button>

    </template>

  </div>

</template>

<script>
  import ScoreBoard from "./components/ScoreBoard.vue";

  export default {

    name: 'App',
    components: {
      ScoreBoard
    },

    data() {
      return {
        question: undefined,
        incorrectAnswar: undefined,
        correctAnswar: undefined,
        chosenAnswer: undefined,
        answarSubimitted: false,
        winCount: 0,
        loseCount: 0
      }
    },

    computed: {
      answars() {
        var answars = JSON.parse( JSON.stringify( this.incorrectAnswar ) );
        answars.splice( Math.round( Math.random() * 4 ), 0, this.correctAnswar );

        return answars;
      }
    },

    methods: {
      submitAnswer() {
        if(!this.chosenAnswer) {
          alert('Escolha uma opção!');
        } else {

          this.answarSubimitted = !this.answarSubimitted;

          if (this.chosenAnswer == this.correctAnswar) {
            this.winCount++;
          } else {
            this.loseCount++;
          }
        }
      },
      getNewQuestion() {

        this.answarSubimitted = false;
        this.chosenAnswer = undefined;
        this.question = undefined;

        this.axios
          .get("https://opentdb.com/api.php?amount=1&category=10&difficulty=easy&type=boolean")
          .then((response) => {
            this.question = response.data.results[0].question;
            this.incorrectAnswar = response.data.results[0].incorrect_answers;
            this.correctAnswar = response.data.results[0].correct_answer;
          })
      }
    },

    created() {
      this.getNewQuestion()
    }
  }

</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px auto;
  max-width: 900px;

  input[type=radio] {
    margin: 12px 4px
  }

  button.send {
    margin-top: 12px;
    min-width: 120px;
    background-color: #1867c0;
    border: 1px solid #1867c0;
    color: #fff;
    cursor: pointer;
    text-align: center;
    height: 40px;
  }
}
</style>