<script setup lang="ts">
import { ref } from 'vue';
import AddPlayers from './components/AddPlayers.vue';
import ShowBoard from './components/ShowBoard.vue';
import { Player } from './models/Player';

let gameOn = ref<boolean>(false);

function toggleGameOn() {
  gameOn.value = true;
}

function quitGame() {
  gameOn.value = false;
  let playerList: Player[] = JSON.parse(
    localStorage.getItem("players") || "[]"
  );
  playerList = [];
  console.log("PlayerList is now empty: ", playerList);
}
</script>

<template>
  <AddPlayers v-if="gameOn === false" @start-game="toggleGameOn" />
  <ShowBoard v-else @quit-game="quitGame" />
</template>

<style scoped></style>
