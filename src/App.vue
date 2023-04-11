<script setup lang="ts">
import { ref } from 'vue';
import AddPlayers from './components/AddPlayers.vue';
import ShowBoard from './components/ShowBoard.vue';

let gameOn = ref<boolean>(false);

function toggleGameOn() {
  gameOn.value = true;
}

</script>

<template>
  <AddPlayers v-if="gameOn === false" @start-game="toggleGameOn" />
  <ShowBoard v-else />
</template>

<style scoped></style>
