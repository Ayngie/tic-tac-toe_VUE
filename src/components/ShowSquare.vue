<script setup lang="ts">

defineProps<{ msg: string }>();

</script>

<template>
    <div class="square">
        <h1>{{ msg }}</h1>
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
