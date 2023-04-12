<script setup lang="ts">
import { onMounted, ref } from 'vue';
import { Player } from '../models/Player';
import ShowSquare from './ShowSquare.vue';
import ResetGame from './ResetGame.vue';

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
</script>

<template>
    <h1> Time to play tic-tac-toe</h1>
    <h2>{{ currentPlayer }} - make your move!</h2>

    <div class="container">
        <div class="board">
            <ShowSquare :msg="playerSymbol" @click.once="handleClick(index)" v-for="(square, index) in 9" :key="square" />
        </div>

    </div>

    <ResetGame />
</template>

<style scoped>
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

h1 {
    color: green;
}
</style>
