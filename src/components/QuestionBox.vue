<template>
    <div class="question-box-container">
        <b-jumbotron>
            <template slot="lead">
                <div>
                    {{ htmlDecode(currentQuestion.question) }}
                </div>
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item 
                href="#" 
                v-for="(answer, index) in currentAnswers" 
                :key="index"
                v-on:click="selectAnswer(index)"
                :class="{
                    selected: index === selectedAnswerIndex,
                    correct: answered && answer === currentQuestion.correct_answer,
                    incorrect: answered && index === selectedAnswerIndex && answer !== currentQuestion.correct_answer
                }"
                >
                    {{ htmlDecode(answer) }}
                </b-list-group-item>
            </b-list-group>

            <b-button 
            variant="primary" 
            href="#"
            :disabled="selectedAnswerIndex === null || answered"
            @click="submitAnswer"
            >
                Submit
            </b-button>
            <b-button 
            variant="success" 
            href="#"
            @click="showNextQuestion"
            :disabled="!answered"
            >
                {{ index !== questions.length - 1 ? 'Next' : 'Show Results' }}
            </b-button>
        </b-jumbotron>
    </div>
</template>

<script>
    export default {
        name: "QuestionBox",
        props: {
            currentQuestion: Object,
            currentAnswers: Array,
            showNextQuestion: Function,
            selectAnswer: Function,
            submitAnswer: Function,
            selectedAnswerIndex: Number,
            answered: Boolean,
            index: Number,
            questions: Array
        },
        methods: {
            htmlDecode: function (input) {
                var doc = new DOMParser().parseFromString(input, "text/html");
                return doc.documentElement.textContent;
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

    .selected {
        background: #007bff;
    }

    .correct {
        background: lightgreen;
    }

    .incorrect {
        background: red;
    }

    .selected,
    .correct,
    .incorrect {
        color: #fff;
    }
</style>
