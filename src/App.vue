<script setup lang="ts">
import { ref } from 'vue';
import AddPlayers from './components/AddPlayers.vue';
import ShowBoard from './components/ShowBoard.vue';
import { Player } from './models/Player';

let gameOn = ref<boolean>(false);
let playerList = ref<Player[]>([]); //vi skapar ett state för vår lista som emittas hit från AddPlayer

function toggleGameOn(players: Player[]) { //här kommer listan med från AddPlayers -emiten.
  gameOn.value = true;
  playerList.value = players; //tilldelar statet värdet av listan vi tog emot
}

function quitGame() {
  gameOn.value = false;
  playerList.value = [];
  console.log("PlayerList is now empty: ", playerList.value);
}
</script>

<template>
  <AddPlayers v-if="gameOn === false" @start-game="toggleGameOn" />
  <ShowBoard v-else @quit-game="quitGame" :players="playerList" />
</template>