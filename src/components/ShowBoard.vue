<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { Player } from '../models/Player';
import ShowSquare from './ShowSquare.vue';

//import list:
let playerList: Player[] = JSON.parse(
    localStorage.getItem("players") || "[]"
);

onMounted(() => {
    console.log(playerList[0])
    console.log(playerList[1])
    // let playerX = playerList[0];
    // let playerY = playerList[1];
});

//set current player:
let currentPlayer = ref<string>("Player X");

//set square symbol
let playerSymbol = ref<string>("");


function handleClick(i: number) {
    console.log("You clicked square: ", i)
    playerSymbol.value = "X"

}

//play again:
function playAgain() {
    console.log("You clicked the button 'Play again'!")
}

//quit game:
let emit = defineEmits(["quitGame"])

function quitGame() {
    emit("quitGame")
}

</script>

<template>
    <h1> Time to play tic-tac-toe</h1>
    <h2>{{ currentPlayer }} - make your move!</h2>

    <div class="container">
        <div class="board">
            <ShowSquare :msg="playerSymbol" @click.once="handleClick(index)" v-for="(square, index) in 9" :key="square" />
        </div>
    </div>

    <div class="handle-game-buttons">
        <button type="button" @click.once="playAgain()"> Play again
        </button>
        <button type="button" @click.once="quitGame()"> Quit game
        </button>
    </div>
</template>

<style scoped>
h1 {
    color: green;
}

.container {
    display: flex;
    justify-content: center;
    align-items: center;
}

.board {
    display: flex;
    flex-wrap: wrap;
    width: 500px;
}

.square {
    margin: 2px;
}

.square:hover {
    cursor: pointer;
}

.square:active {
    background-color: green;
    color: white;
}

.handle-game-buttons {
    padding: 15px;
}

button {
    margin: 10px;
}

button:hover {
    border-color: green;
}
</style>
