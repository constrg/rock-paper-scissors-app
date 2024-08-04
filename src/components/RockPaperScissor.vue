<script setup>
import {ref} from 'vue';

const props = defineProps({
    title: {
        type: String,
    }
});

const choices = ['✊', '✋', '✌️'];
const result = ref('Make your move');
const choiceButtonDisabled = ref(false);
const tryAgainButton = ref(false);
const userScore = ref(0);
const computerScore = ref(0);

const TIE_MESSAGE = "It's a tie!";
const USER_WIN_MESSAGE = "You Win!";
const COMPUTER_WIN_MESSAGE = "Computer Wins!";
const GAME_END_USER_WIN_MESSAGE = 'You Win the Game!';
const GAME_END_COMPUTER_WIN_MESSAGE = 'Computer Wins the Game!';

const determineWinner = (user, computer) => {
    if(user === computer) {
        return TIE_MESSAGE;
    }
    else if (
        (user === '✊' && computer === '✌️') ||
        (user === '✋' && computer === '✊') ||
        (user === '✌️' && computer === '✋')
    ) {
        userScore.value += 1;
        return USER_WIN_MESSAGE;
    }
    else {
        computerScore.value += 1;
        return COMPUTER_WIN_MESSAGE;
    }
}

const checkGameEnd = () => {
    if(userScore.value === 5) 
    {
        result.value = GAME_END_USER_WIN_MESSAGE;
        tryAgainButton.value = true;
        return true;
    }
    else if(computerScore.value === 5)
    {
        result.value = GAME_END_COMPUTER_WIN_MESSAGE;
        tryAgainButton.value = true;
        return true;
    }

    return false;
}

const play = (userChoice) => {
    const computerChoice = choices[Math.floor(Math.random() * choices.length)];
    result.value = `You chose ${userChoice}, Computer chose ${computerChoice}. ${determineWinner(userChoice, computerChoice)}`;
    choiceButtonDisabled.value = true;

    if (checkGameEnd()) return;

    setTimeout(() => {
        result.value = 'Make your move';
        choiceButtonDisabled.value = false;
    }, 3000);
}

const tryAgainHandler = () => {
    result.value = 'Make your move';
    tryAgainButton.value = false;
    choiceButtonDisabled.value = false;
    userScore.value = 0;
    computerScore.value = 0;
}
</script>
<template>
    <div class="rock-paper-scissors">
        <div class="container">
            <p class="game-instructions">First to score 5 points wins!</p>
            <div class="score-board">
                <span class="user">You: {{ userScore }}</span>
                <span class="computer">Computer: {{ computerScore }}</span>
            </div>
            <h1 class="title">{{ title }}</h1>
            <h2 class="result">{{result}}</h2>
            <button class="try-again-button" v-show="tryAgainButton" @click="tryAgainHandler">Try again</button>
            <div class="choices">
                <button class="choice-button" @click="play('✊')" :disabled="choiceButtonDisabled"><span class="choice-button-emoji">✊</span></button>
                <button class="choice-button" @click="play('✋')" :disabled="choiceButtonDisabled"><span class="choice-button-emoji">✋</span></button>
                <button class="choice-button" @click="play('✌️')" :disabled="choiceButtonDisabled"><span class="choice-button-emoji">✌️</span></button>
            </div>
        </div>
    </div>
</template>

<style scoped>

.rock-paper-scissors
{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: center;
    min-height: 100vh;
    padding: 20px;
}

.rock-paper-scissors .game-instructions
{
    font-size: 1rem;
    margin-bottom: 10px;
}


.rock-paper-scissors .score-board
{
    padding: 10px;
    background-color: #ffffff;
    max-width: 200px;
    margin: 0 auto;
    color: #242424;
    display: flex;
    align-items: center;
    column-gap: 20px;
    font-weight: 700;
    border-radius: 5px;
    margin-bottom: 20px;
    font-size: 1rem;
}

.rock-paper-scissors .title 
{
    margin-bottom: 20px;
}

.rock-paper-scissors .result
{
    margin-bottom: 20px;
    color: #ffd500;
}

.rock-paper-scissors .try-again-button
{
    margin-bottom: 40px;
    background-color: #13df4d;
    color: #ffffff;
    font-weight: 500;
    font-size: 1rem;
}

.rock-paper-scissors .try-again-button:hover{
    animation: wiggle 0.3s ease-in-out;
}

.rock-paper-scissors .choices 
{
    display: flex;
    justify-content: center;
    align-items: center;
    column-gap: 15px;
}
.rock-paper-scissors .choices .choice-button
{
    font-size: 2.5rem;
    background-color: #181818;
    border: 1px solid #131313;
}

.rock-paper-scissors .choices .choice-button:hover
{
    animation: wiggle 0.3s ease-in-out;
}

.rock-paper-scissors .choices .choice-button:disabled 
{
    cursor: not-allowed;
}

@keyframes wiggle {
    0%, 100% {
        transform: rotate(5deg);
    }
    50% {
        transform: rotate(-5deg);
    }
}

</style>