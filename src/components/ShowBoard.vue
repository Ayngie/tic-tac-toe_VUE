<script setup lang="ts">
import { ref } from 'vue';
import ShowSquare from './ShowSquare.vue';

let state = ref<string[]>([]);
let currentPlayer = ref<string>("PlayerX");
</script>

<template>
    <h1> Time to play tic-tac-toe</h1>
    <h2>{{ currentPlayer }} - make your move!</h2>

    <div class="container">
        <div class="board">
            <ShowSquare v-for="n in 9" />
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
