<template>
  <div class="ui raised very padded text container segment">
    <h2 class="ui header">Question {{ questionCount+1 }}</h2>
    <p>{{ currentQuestion.question }}</p>
    <h4 class="ui horizontal divider header">
      Answers
    </h4>
    <table class="ui definition table">
      <tbody>
        <tr 
        v-for="(answer, index) in answers" :key="index"
        @click.prevent="selectAnswer(index)"
        :class="answerClass(index)"
       
        >
          <td class="two wide column">{{ index+1 }}</td>
          <td>{{ answer }}</td>
        </tr>
      </tbody>
    </table>

<div class="ui button" tabindex="0" @click="previous">
  &lt; Previous
</div>
<div class="ui button" tabindex="0" @click="next">
  Next &gt;
</div>

<button 
class="ui primary button"
@click="submitAnswer"
:disabled="selectedAnswer === null || answered == true"
>
  Submit
</button>
  </div>
</template>

<script>
import _ from 'lodash'
export default {
    props:{
        currentQuestion:Object,
        questionCount:Object,
        next:Function,
        previous:Function,
        increment:Function
    },
    data(){
      return {
        selectedAnswer:null,
        correctAnswer:null,
        shuffledAnswers:[],
        answered:false
      }
    },
    methods:{
      selectAnswer(index){
        this.selectedAnswer = index
      },
      shuffleAnswers(){
        let answers = [...this.currentQuestion.incorrect_answers, this.currentQuestion.correct_answer ]
        this.shuffledAnswers = _.shuffle(answers)
        this.correctAnswer = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
      },
      submitAnswer(){
        let isCorrect = false;
        this.answered = true
        if (this.selectedAnswer == this.correctAnswer){
            isCorrect = true;
        }
        this.increment(isCorrect);
      },
      answerClass(index){
          let answerClass = ''
          let answered = this.answered;
          let selectedAnswer = this.selectedAnswer;
          let correctAnswer = this.correctAnswer
          if(!answered && selectedAnswer === index ){
            answerClass = 'selected'
          }else if(answered && correctAnswer === index){
            answerClass = 'correct'
          }else if( answered && selectedAnswer === index && correctAnswer !== index ){
             answerClass = 'wrong'
          }
          return answerClass
      }
    },
    computed:{
        answers(){
            let answers = [...this.currentQuestion.incorrect_answers]
            answers.push(this.currentQuestion.correct_answer)
            return answers
        }
    },
    watch:{
      currentQuestion:{
        immediate:true,
        handler(){
        this.selectedAnswer = null
        this.shuffleAnswers()
        this.answered = false 
        }
      }
    }
}
</script>

<style scoped>
  tr:hover{
    background: rgba(119, 117, 117, 0.356);
    cursor:pointer;
  }

  .selected{
    background: #2184d088;
  }
  .correct{
    background: rgba(0, 128, 85, 0.37);
  }

  .wrong{
    background: rgba(206, 21, 67, 0.425);
  }
</style>