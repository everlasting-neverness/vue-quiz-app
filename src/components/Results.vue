<template>
    <b-container 
    class="bv-example-row"
    >
        <h3>Your Results</h3>
        <b-list-group class="results">
            <b-list-group-item>
                <span>Correct Answers: </span><strong>{{ numCorrect }}</strong>
            </b-list-group-item>
            <b-list-group-item>
                <span>Total Answers: </span><strong>{{ numTotal }}</strong>
            </b-list-group-item>
        </b-list-group>
         <b-button 
            variant="primary" 
            href="#"
            @click="startQuiz"
            >
                Start New Quiz
            </b-button>
    </b-container> 
</template>

<script>
    export default {
        name: "Results",
        props: {
            numCorrect: Number,
            numTotal: Number,
            startQuiz: Function
        }
    }
</script>

<style scoped>
    h3 {
        margin-top: 15px;
    }

    .results {
        margin-bottom: 15px;
    }
</style>


