<template>
  <div class="question-box-container">
    <b-jumbotron>

            <template v-slot:lead>
                {{currentQuestion.question}}
                
            </template>

            <hr class="my-4">

            <b-list-group>
                <b-list-group-item 
                    v-for="(answer , index) in shuffledAnswers" :key="index" 
                    @click.prevent="selectAnswer(index)"
                    :class="[selectedIndex === index ? 'selected' : '']"
                    > 
                    
                    {{answer}}
                </b-list-group-item>

            </b-list-group>

            <b-button variant="primary" @click="submitAnswer">Submit</b-button>
            <b-button @click="next" variant="success" href="#">Next</b-button>

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
        return {
            selectedIndex:null,
            correct_Index:null,
            shuffledAnswers:[]
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
            immediate: true,
            handler(){
                this.selectedIndex = null;
                this.shuffleAnswers();
            }
        }
    },
    methods:{
        selectAnswer(index){
            this.selectedIndex = index;
            console.log(index);
            
        },
        shuffleAnswers(){
            let answers = [...this.currentQuestion.incorrect_answers , this.currentQuestion.correct_answer]   
            this.shuffledAnswers = _.shuffle(answers)
            this.correct_Index = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer)
            
        },
        submitAnswer(){
            let isCorrect = false
            
            if (this.selectedIndex === this.correct_Index) {
                isCorrect = true
                
            }
            this.increment(isCorrect)
        }
    },

}
</script>

<style scoped>

.list-group{
    margin:15px;
}
.list-group-item:hover{
    background: #eee;
    cursor: pointer;
}
.btn{
    margin: 3px;
}
.selected{
    background-color : lightblue;
}
.correct{
    background-color: green;
}
.incorrect{
    background-color: red;
}
</style>