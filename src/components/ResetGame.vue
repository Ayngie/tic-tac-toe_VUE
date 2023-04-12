<script setup lang="ts">

//reset game:
function resetGame() {
    localStorage.clear;
}
</script>

<template>
    <div>
        <button type="button" @click.once="resetGame()"> Reset game
        </button>

    </div>
</template>

<style scoped>
div {
    display: flex;
    justify-content: center;
    align-items: center;
}

.button {
    margin: 2px;
}

.button:hover {
    cursor: pointer;
}

.button:active {
    background-color: green;
    color: white;
}
</style>
