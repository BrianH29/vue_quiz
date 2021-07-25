<template>
  <div id="app">
    <QuizHeader/>
    <br>
    <QuestionBox/>
  </div>
</template>

<script>
import QuizHeader from './components/QuizHeader.vue'
import QuestionBox from './components/QuestionBox.vue'

export default {
  components : {
    QuizHeader,
    QuestionBox,
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
