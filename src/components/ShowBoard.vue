<script setup lang="ts">
import { ref } from 'vue';
import ShowSquare from './ShowSquare.vue';
import { Player } from '../models/Player';

let playerList: Player[] = JSON.parse(
    localStorage.getItem("players") || "[]"
);
console.log(playerList)

// let playerX = playerList.values[0];

let currentPlayer = ref<string>("PlayerX");

function handleClick(i: number) {
    console.log("You clicked square: ", i)
}
</script>

<template>
    <h1> Time to play tic-tac-toe</h1>
    <h2>{{ currentPlayer }} - make your move!</h2>

    <div class="container">
        <div class="board">
            <ShowSquare @click.once="handleClick(index)" v-for="(square, index) in 9" :key="square" />
        </div>

    </div>
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

h1 {
    color: green;
}
</style>
