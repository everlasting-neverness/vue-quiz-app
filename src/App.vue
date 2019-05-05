<template>
  <div id="app">
    <Header 
    :numCorrect="numCorrect"
    :numTotal="numTotal"
    />
    <b-container 
    class="bv-example-row app-container"
    >
      <b-row>
        <b-col sm="6" offset="3">
          <QuestionBox 
          v-if="questions.length && index !== questions.length"
          :showNextQuestion="showNextQuestion" 
          :selectAnswer="selectAnswer"
          :selectedAnswerIndex="selectedAnswerIndex"
          :submitAnswer="submitAnswer"
          :currentQuestion="questions[index]"
          :currentAnswers="currentAnswers"
          :answered="answered"
          :index="index"
          :questions="questions"
          />
          <Results 
          v-show="index === questions.length" 
          :numCorrect="numCorrect"
          :numTotal="numTotal"
          :startQuiz="startQuiz"
          />
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
  import Header from './components/Header.vue';
  import QuestionBox from './components/QuestionBox.vue';
  import Results from './components/Results.vue';

  export default {
    name: 'app',
    components: {
      Header,
      QuestionBox,
      Results
    },
    data() {
      return {
        questions: [],
        currentAnswers: [],
        index: 0,
        selectedAnswerIndex: null,
        answered: false,
        numCorrect: 0,
        numTotal: 0
      }
    },
    mounted() {
      this.getData();
    },
    methods: {
      getData: function(reset = false) {
        fetch('https://opentdb.com/api.php?amount=10&category=11&type=multiple', {
          method: "GET"
        }).then(response => {
          return response.json();
        }).then(jsonData => {
          this.questions = jsonData.results;
          if (reset) {
            this.resetData();
          }
          this.prepareAnswers();
        })
      },
      resetData: function() {
        this.selectedAnswerIndex = null;
        this.answered = false;
        this.index = 0;
        this.numCorrect = 0;
        this.numTotal = 0;
      },
      showNextQuestion: function() {
        this.index++;
        if (this.index === this.questions.length) {
          return false;
        }
        this.selectedAnswerIndex = null;
        this.answered = false;
        this.prepareAnswers();
      },
      selectAnswer: function(answerIndex) {
        this.selectedAnswerIndex = answerIndex;
      },
      submitAnswer: function(e) {
        e.preventDefault();
        this.answered = true;
        let isCorrect = false;
        
        if (this.currentAnswers[this.selectedAnswerIndex] === this.questions[this.index].correct_answer) {
          isCorrect = true;
        }
        
        this.incrementCounters(isCorrect);
      },
      incrementCounters: function(isCorrect) {
        isCorrect && this.numCorrect++;
        this.numTotal++;
      },
      prepareAnswers: function() {
          this.currentAnswers = [...this.questions[this.index].incorrect_answers, this.questions[this.index].correct_answer]
              .sort(() => Math.random() - 0.5);
      },
      startQuiz: function() {
        this.getData(true);
      }
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
  
  .app-container {
    margin-top: 15px;
  }
</style>
