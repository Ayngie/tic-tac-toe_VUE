<script setup lang="ts">
import { ref } from 'vue';
import ShowSquare from './ShowSquare.vue';

let state = ref<string[]>([]);
</script>

<template>
    <h1> Welcome to this game of tic-tac-toe</h1>

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
