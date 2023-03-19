<template>

  <div>
    <template v-if="this.question">
        <h1 v-html="this.question"></h1>

      <template v-for="answar in this.answars" :key="answar">

        <input type="radio" name="options" value="answar">
        <label v-html="answar"></label><br>

      </template>
    </template>

    <button class="send" type="button">enviar</button>
  </div>

</template>

<script>

  export default {
    name: 'App',

    data() {
      return {
        question: undefined,
        incorrectAnswars: undefined,
        correctAnswars: undefined
      }
    },

    computed: {
      answars() {
        var answars = JSON.parse( JSON.stringify( this.incorrectAnswars ) );
        answars.splice( Math.round(Math.random() * answars.lenght ), 0, this.correctAnswars );

        return answars;
      }
    },

    created() {
      this.axios
        .get("https://opentdb.com/api.php?amount=1&type=multiple")
        .then((response) => {
          this.question = response.data.results[0].question;
          this.incorrectAnswars = response.data.results[0].incorrect_answers;
          this.correctAnswars = response.data.results[0].correct_answer;
        })
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