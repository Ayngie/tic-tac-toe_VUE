<script setup lang="ts">

//import list:
interface IShowSquareProps {
    symbol: string
}
defineProps<IShowSquareProps>();

</script>

<template>
    <div class="square">
        <h1>{{ symbol }}</h1>
    </div>
</template>

<style scoped>
.square {
    display: flex;
    justify-content: center;
    align-items: center;
    border: 2px solid green;
    height: 150px;
    width: 150px;
}
</style>
