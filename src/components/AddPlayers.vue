<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';

let state = ref<Player>({ name: "", playerWins: false });
const players = ref<Player[]>([]);

let playerOneAdded = false;

function handleSubmit() {
    players.value.push(new Player(state.value.name, false));
    state.value = { name: "", playerWins: false } //rensa inputrutorna
    playerOneAdded = true;
    console.log(players.value)

    if (players.value.length === 2) {
        console.log("There are now two players")
        //emit starta spel gameon till true
    }
}
</script>

<template>
    <h1> Welcome to this game of tic-tac-toe!</h1>
    <h2> Enter player names to start:</h2>

    <form @submit.prevent="handleSubmit" v-if="playerOneAdded">
        <div class="inputRows">
            <input v-model="state.name" type="text" placeholder="Type name here..." />
            <button type="submit" class="submitBtn">Add player 2</button>
        </div>
    </form>

    <form @submit.prevent="handleSubmit" v-else>
        <div class="inputRows">
            <input v-model="state.name" type="text" placeholder="Type name here..." />
            <button type="submit" class="submitBtn">Add player 1</button>

        </div>
    </form>

    <div v-if="playerOneAdded">
        <p>Game participants:</p>
        <div v-for="player in players" class="participant-names"> {{ player.name }} </div>
    </div>
</template>

<style scoped>
form {
    display: flex;
    justify-content: center;
}

.inputRows {
    display: flex;
    justify-content: space-around;
    align-items: center;
    width: 35%;
}

input {
    padding: 5px;
}

h1 {
    color: green;
}

.participant-names {
    font-style: italic;
}
</style>
