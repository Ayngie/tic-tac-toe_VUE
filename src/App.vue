<script setup lang="ts">
import { ref } from 'vue';
import AddPlayers from './components/AddPlayers.vue';
import ShowBoard from './components/ShowBoard.vue';
import { Player } from './models/Player';

//variables
let gameOn = ref<boolean>(false);
let playerList = ref<Player[]>([]); //vi skapar ett state för vår lista som emittas hit från AddPlayer

// Kolla om spel är igång, isåfall ska gameOn köras med spelarlista från localStorage
let storedPlayersList: Player[] = JSON.parse(
  localStorage.getItem("storedPlayers") || "[]"
);

if (storedPlayersList.length > 0) {
  playerList.value = storedPlayersList;
  // console.log("Updated playerList is:", playerList.value);
  toggleGameOn(playerList.value);
}

//Nytt spel:
function toggleGameOn(players: Player[]) { //här kommer listan med från AddPlayers -emiten.
  gameOn.value = true;
  playerList.value = players; //tilldelar statet värdet av listan vi tog emot
  localStorage.setItem("storedPlayers", JSON.stringify(playerList.value)); //Set players to localStorage:
}

//Avsluta spel
function quitGame() {
  gameOn.value = false;
  playerList.value = [];
  // console.log("PlayerList is now empty: ", playerList.value);
}
</script>

<template>
  <AddPlayers v-if="gameOn === false" @start-game="toggleGameOn" />
  <ShowBoard v-else @quit-game="quitGame" :players="playerList" :gameOn="gameOn" />
</template>

<style scoped></style>
