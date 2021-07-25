<template>
  <div id="app">
    <QuizHeader
      :correctNum="correctNum"
      :totalNum="totalNum"
    />
    <br>
    <QuestionBox
      v-if="questions.length"
      :currentQuestion="questions[index]"
      :next="next"
      v-on:increment="increment"
    />
  </div>
</template>

<script>
import QuizHeader from './components/QuizHeader.vue'
import QuestionBox from './components/QuestionBox.vue'
import axios from 'axios'

export default {
  components : {
    QuizHeader,
    QuestionBox,
  },
  data() {
    return {
      questions: [],
      index : 0, 
      correctNum : 0,
      totalNum : 0,
    }
  },
  methods: {
    next(){
      this.index++
    },
    increment(isCorrect){
      if(isCorrect){
        this.correctNum++
      }
      this.totalNum++
    }
  },
  created() {
    axios.get('https://opentdb.com/api.php?amount=10&category=23&difficulty=easy')
    .then(response => {
      return this.questions = response.data.results; 
    })
    .catch(err => console.log(err))
  }
}
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
