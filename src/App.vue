<template>
  <div id="app">  
    <div v-if="totalAnswer == questions.length">
      <b-modal id="modal-1" no-close-on-backdrop title="Your Result" @ok="saveData">
        <p class="my-4">Total Questions: <b>{{totalAnswer}}</b>!</p>
        <p class="my-4">Total Correct Answer is: <b>{{correctAnswer}}</b>!</p>
      </b-modal>
    </div>
    <Header :correctAnswer='correctAnswer' :totalAnswer="totalAnswer" />
    <b-container class="bv-example-row">
      <b-row>
        <b-col v-if="questions.length > 0" cols="6" offset="3">
          <questionBox :increment='increment' :currentQuestion="questions[index]" :changeQuestion="changeQuestion"/>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import questionBox from './components/questionBox.vue'

export default {
  name: 'App',
  components: {
    Header,
    questionBox
  },
  data(){
    return {
      questions: [],
      index: 0,
      correctAnswer: 0,
      totalAnswer: 0
    }
  },
  methods: {
    saveData: function(){
      window.location.reload()
    },
    changeQuestion: function(){
      console.log('fffffffffffffff',this.totalAnswer);
      if( this.index+1 != this.questions.length ){
        this.index++;
      }
    },
    increment: function(flag){
      if(flag){
        this.correctAnswer++;
      }
      this.totalAnswer++;
    }
  },
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&category=18&type=multiple',{
      method:'get'
    })
    .then(res => {
      return res.json();
    })
    .then(res => {
      this.questions = res.results;      
      console.log('fff',res.results);
    })
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
