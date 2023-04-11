<script setup lang="ts">
import AddPlayers from './components/AddPlayers.vue';
import ShowBoard from './components/ShowBoard.vue';

let gameOn: boolean = false;

</script>

<template>
  <AddPlayers v-if="gameOn === false" />
  <ShowBoard v-else />
</template>

<style scoped></style>
