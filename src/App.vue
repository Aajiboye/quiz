<template>
  <div id="app">
    <Header
    :numCorrect="numCorrect"
    :numTotal="numTotal"
     />
    <QuestionBox  
    v-if = "questions.length"
    :currentQuestion="questions[index]"
    :questionCount="index"
    :next="next"
    :previous="previous"
    :increment="increment"/>
  </div>
</template>

<script>
import Header from './components/Header.vue'
import QuestionBox from './components/QuestionBox.vue'
export default {
  name: 'App',
  components: {
    Header,
    QuestionBox
  },
  data(){
    return {
      questions:[],
      index:0,
      numCorrect:0,
      numTotal:0
    }
  },
  methods:{
    next(){
      if(this.index < this.questions.length) this.index ++
    },
    previous(){
      if(this.index > 0) this.index --
    },
    increment(isCorrect){
      if(isCorrect){
        this.numCorrect ++
      }
        this.numTotal ++
    }

  },
  mounted: function(){
    fetch('https://opentdb.com/api.php?amount=10&category=19&type=multiple',
    {method:'get'})
    .then(response=>{
       return response.json();
    })
    .then(resData =>{
        this.questions =  resData.results
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
