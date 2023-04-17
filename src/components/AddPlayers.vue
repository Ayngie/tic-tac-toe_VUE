<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';

//variables
const players = ref<Player[]>([]);
let state = ref<Player>({ name: "", symbol: "", score: 0 });
let playerOneAdded = false;

//emit
let emit = defineEmits(["startGame"])


//Add players
function handleSubmit() {
    players.value.push(new Player(state.value.name, players.value.length === 0 ? "X" : "O", state.value.score));
    state.value = { name: "", symbol: "", score: 0 } //rensa inputrutorna
    playerOneAdded = true;

    if (players.value.length === 2) {
        //emit starta spel (som kommer ändra gameOn till true) 
        console.log("There are now two players, start game!")
        console.log("Player 1 is: ", players.value[0])
        console.log("Player 2 is: ", players.value[1])

        emit("startGame", players.value) //här i anropet skickar vi med vad vi vill emitta till förädern.
    }
}
</script>

<template>
    <h1> Let's play tic-tac-toe!</h1>
    <h2> Enter player names to start:</h2>

    <form @submit.prevent="handleSubmit" v-if="playerOneAdded === false">
        <div class="inputRows">
            <input v-model.trim="state.name" type="text" placeholder="Type name here..." />
            <button type="submit" class="button">Add player 1</button>

        </div>
    </form>

    <form @submit.prevent="handleSubmit" v-else>
        <div class="inputRows">
            <input v-model.trim="state.name" type="text" placeholder="Type name here..." />
            <button type="submit" class="button">Add player 2</button>
        </div>
    </form>


    <div v-if="playerOneAdded">
        <span class="participant-header">Game participants: </span>
        <span v-for="player in players" class="participant-names"> {{ player.name + ", " }} </span>
    </div>
</template>

<style scoped>
h1 {
    color: green;
}

form {
    display: flex;
    justify-content: center;
}

.inputRows {
    display: flex;
    justify-content: space-around;
    align-items: center;
    width: 65%;
}

input {
    padding: 5px;
    color: green;
}

input:hover {
    border-color: green;
    border-radius: 3px;
}

input:focus {
    outline: 2px solid green;
    border-radius: 3px;
    border: none;
}


.button {
    margin: 10px;
    background-color: rgb(221, 255, 221);
}

.button:hover {
    border-color: green;
}

.button:focus {
    outline: 2px solid green;
}

.participant-header {
    color: green;
    font-weight: bold;
}

.participant-names {
    color: green;
    font-style: italic;
}
</style>
