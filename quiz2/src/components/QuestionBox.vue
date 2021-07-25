<template>
  <div>
    <b-jumbotron>
      <template slot=lead> {{ currentQuestion.question }} </template>

      <hr class="my-4">

      <b-list-group>
        <b-list-group-item 
          v-for="(answer,index) in shuffledAnswers" 
          :key="index"
          :class="checkAnswer(index)"
          @click="selectAnswer(index)">
            {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex === null || answered">submit</b-button>
      <b-button variant="success" @click="next" :disabled="!answered">next</b-button>
  </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash';

export default {
  props:{
    currentQuestion : Object,
    next : Function,
    increment : Function
  },
  data(){
    return{
      selectedIndex : null,
      shuffledAnswers : [],
      answered: false,
      correctIndex : null
    }
  },
  computed:{
    answers(){
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer);
      return answers
    }
  },
  watch:{
    currentQuestion : {
      immediate: true,
      handler(){
        this.selectedIndex = null; 
        this.answered = false; 
        this.shuffleAnswers()
      }
    }
  },
  methods:{
    selectAnswer(index){
      this.selectedIndex = index
    },
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers)
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer(){
      let isCorrect = false

      if(this.selectedIndex === this.correctIndex){
        isCorrect = true;
      }
      this.answered = true;
      this.$emit('increment',isCorrect)
    },
    checkAnswer(index){
      let answerCheck = ''

      if(!this.answered && this.selectedIndex === index){
        answerCheck = 'selected'
      } else if(this.answered && this.correctIndex === index){
        answerCheck = 'correct'
      } else if(this.answered && this.selectedIndex === index && this.correctIndex !== index){
        answerCheck = 'inCorrect'
      }
      console.log("what is answerCheck",answerCheck);
      return answerCheck 
    }
  }

}
</script>

<style scoped>
.list-group {
  margin-bottom: 15px;
}
.btn {
  margin: 0 5px;
}
.list-group-item:hover {
  background: #eee;
  cursor: pointer;
}
.selected {
  background-color: lightblue;
}
.correct {
  background-color: lightgreen;
}
.inCorrect {
  background-color: rgb(240, 61, 61);
}
</style>