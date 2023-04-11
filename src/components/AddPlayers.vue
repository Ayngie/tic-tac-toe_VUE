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
}
</script>

<template>
    <h3>Welcome</h3>
<form @submit.prevent="handleSubmit" v-if="playerOneAdded">
        <div>
            <input v-model="state.name" type="text" placeholder="Type name here..." />
            <button type="submit" class="submitBtn">Add player 2</button>

        </div>
    </form>
    <form @submit.prevent="handleSubmit" v-else>
        <div>
            <input v-model="state.name" type="text" placeholder="Type name here..." />
            <button type="submit" class="submitBtn">Add player 1</button>

        </div>
    </form>
    <div>
        <!-- <p>{{ state }}</p>
                <p>{{ playerOneAdded }}</p> -->
        <p>Game participants:</p>
        <div v-for="player in players"> {{ player.name }} </div>
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
