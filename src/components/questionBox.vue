<template>
   <div class="question-box-countainer">
    <b-jumbotron>
       <template #lead>
        {{ currentQuestion.question }}
        </template>

        <hr class="my-4">
        <b-list-group>
        <b-list-group-item v-for="(mcq, index) in randomizeMCQs" :key="index" @click="selectAnswer(index)" :class="answerClass(index)"> {{mcq}}</b-list-group-item>
        </b-list-group>

        <b-button variant="primary"  :disabled="selectedIndex === null || answered" @click="submit" href="#">Submit</b-button>
        <b-button variant="success"  @click="changeQuestion" v-b-modal.modal-1 href="#">Next</b-button>    
    </b-jumbotron>
   </div>
</template>

<script>
import _ from "lodash"

export default {
    props: {
        currentQuestion: Object,
        changeQuestion: Function,
        increment: Function
    },
    data(){
       return {
           selectedIndex: null,
           correctIndex: '',
           answered: false,
           shuffledAnswers: []
       }
    },
    computed:{
        randomizeMCQs: function(){
            return [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
        }
    },
    watch: {
        currentQuestion:{
            immediate: true,
            handler(){
                this.answered = false
                this.selectedIndex = null
                this.shuffleAnswer()               
            }
        }
    },
    methods: {
        answerClass(index){
            if( this.selectedIndex == index && !this.answered ){
                return 'selected'
            }
            if( this.answered && this.correctIndex == index ){
                return 'correct'
            }
            if( this.answered && this.selectedIndex == index ){
                return 'incorrect'
            }
        },
        submit(){
            this.answered = true
            let isCorrect = false
            if( this.selectedIndex == this.correctIndex ){
                isCorrect = true
            }
            this.increment(isCorrect)
        },
        selectAnswer(index){
            this.selectedIndex = index;
        },
        shuffleAnswer(){
          let answer =  [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer]
          this.shuffledAnswers = _.shuffle(answer);
          this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
        }
    }
}

</script>

<style scoped>
.list-group {
    margin-buttom: 15px;
} 
.btn {
    margin: 0 5px;
}
.selected {
   background-color: #7070d6;
}
.correct {
    background: #65bf65;
}
.incorrect {
    background: #e45656;
}
.list-group-item:hover {
    background: #eee;
    cursor: pointer;
}
</style>