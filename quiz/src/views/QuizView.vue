<script setup>
import Question from '../components/Question.vue';
import QuizHeader from '../components/QuizHeader.vue';
import Results from '../components/Results.vue';
import quizes from '../data/quizes.json';
import { ref, computed } from 'vue';
import { useRoute } from 'vue-router';

const route = useRoute()
const quizId = parseInt(route.params.id)
const currentQuestionIndex = ref(0)
const quiz = quizes.find(quiz => quiz.id === quizId)
const numberOfCorrectAnswers=ref(0)
const showResults=ref(false)

// const questionStatus = ref(`${currentQuestionIndex.value}/${quiz.questions.length}`)

// watch(() => currentQuestionIndex.value, () => {
//         questionStatus.value = `${currentQuestionIndex.value}/${quiz.questions.length}`
// })

const questionStatus = computed(() => `${currentQuestionIndex.value}/${quiz.questions.length}`)
const barPercentage = computed(() => `${currentQuestionIndex.value / quiz.questions.length * 100}%`)


const onOptionSelected=(isCorrect)=>{
    console.log('on option selected',isCorrect)
    if(isCorrect){
        numberOfCorrectAnswers.value++
    }
    if(currentQuestionIndex.value === quiz.questions.length-1 ){
        showResults.value=true
        return
    }


    currentQuestionIndex.value++
}
</script>

<template>
    <div>
        <div v-if="!showResults">
            <QuizHeader :questionStatus="questionStatus" :barPercentage="barPercentage" />
        <div>
            <Question :question="quiz.questions[currentQuestionIndex]" @optionSelected="onOptionSelected" />
        </div>

        </div>
        <Results v-else :numberOfCorrectAnswers="numberOfCorrectAnswers" :totalQuestions="quiz.questions.length"/>

    </div>
</template>
