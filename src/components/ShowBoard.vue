<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';
import ShowSquare from './ShowSquare.vue';

//import list:
interface IShowBoardProps {
    players: Player[]
}

const props = defineProps<IShowBoardProps>();

//Variables
let currentPlayer = ref<Player>(props.players[0]);
let xIsPlaying = ref<boolean>(true);
let squares = ref<string[]>(["", "", "", "", "", "", "", "", ""])

function handleClick(i: number) {
    squares.value[i] = currentPlayer.value.role;

    console.log(currentPlayer.value.name, "clicked square: ", i)

    currentPlayer.value.role = xIsPlaying ? "X" : "O"; //fel

    //switch from player 1 to player 2
    if (currentPlayer.value.role === "X") {
        xIsPlaying.value = false

    }
    //switch from player 2 to player 1
    // else {
    //     xIsPlaying.value = true
    // }
}


//play again:
function playAgain() {
    console.log("You clicked the button 'Play again'!")
}

//quit game:
let emit = defineEmits(["quitGame"])

function quitGame() {
    console.log("You clicked the button 'Quit game'!")
    emit("quitGame")
}

</script>

<template>
    <h1> Time to play tic-tac-toe</h1>
    <h2>{{ currentPlayer.name }} - make your move!</h2>

    <div class="container">
        <div class="board">
            <ShowSquare :symbol="squares[index]" @click.once="handleClick(index)" v-for="square, index in squares"
                :key="square" />
        </div>
    </div>

    <div class="handle-game-buttons">
        <button type="button" @click.once="playAgain()"> Play again
        </button>
        <button type="button" @click.once="quitGame()"> Quit game
        </button>
    </div>
</template>

<style scoped>
h1 {
    color: green;
}

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

.square {
    margin: 2px;
}

.square:hover {
    cursor: pointer;
}

.square:active {
    background-color: green;
    color: white;
}

.handle-game-buttons {
    padding: 15px;
}

button {
    margin: 10px;
}

button:hover {
    border-color: green;
}
</style>
