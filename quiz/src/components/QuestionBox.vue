<template>
  <div>
    <b-jumbotron>
      <template slot="lead"> {{ currentQuestion.question }} </template>

      <hr class="my-4" />

      <b-list-group>
        <b-list-group-item 
          v-for="(answer, index) in shuffledAnswers" 
          :key="index" 
          :class="answerClass(index)"
          @click="selectAnswer(index)">
          {{ answer }}
        </b-list-group-item>
      </b-list-group>

      <b-button variant="primary" @click="submitAnswer" :disabled="selectedIndex === null || answered">Submit</b-button>
      <b-button variant="success" @click="next">Next</b-button>
    </b-jumbotron>
  </div>
</template>

<script>
import _ from 'lodash'

export default {
  props: {
    currentQuestion: Object,
    next: Function,
    increment: Function,
  },
  data(){
    return {
      selectedIndex : null,
      shuffledAnswers : [],
      correctIndex : null,
      answered: false
    }
  },
  computed: {
    answers() {
      let answers = [...this.currentQuestion.incorrect_answers];
      answers.push(this.currentQuestion.correct_answer); 
      return answers
    }
  },
  watch : {
    currentQuestion: {
      immediate : true,
      handler: function(){
        this.selectedIndex = null
        this.answered = false
        this.shuffleAnswers()
      }
    }
    
    //아래 mounted 일겨우 
    // currentQuestion() {
    //   this.selectedIndex = null
    //   this.shuffleAnswers()
    // }
  },
  methods:{
    selectAnswer(index) {
      this.selectedIndex = index
    },
    shuffleAnswers(){
      let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
      this.shuffledAnswers = _.shuffle(answers); 
      this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
    },
    submitAnswer(){
      let isCorrect = false

      if(this.selectedIndex === this.correctIndex) {
        isCorrect = true; 
      }
      this.answered = true; 
      this.increment(isCorrect)
    },
    answerClass(index) {
      let answerClass = ''

      if(!this.answered && this.selectedIndex === index){
        answerClass = 'selected'
      } else if (this.answered && this.correctIndex === index){
        answerClass = 'correct'
      } else if (this.answered && this.selectedIndex === index && this.correctIndex !== index) {
        answerClass = 'inCorrect'
      }
      return answerClass

    }
  },
  // 처음 문제가 shuffle이 안되기 때문에 mounted할 때 shuffle 적용 시키기 혹은 위에 watch 에서 하기 
  // mounted() {
  //   this.shuffleAnswers(); 
  // },
};
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