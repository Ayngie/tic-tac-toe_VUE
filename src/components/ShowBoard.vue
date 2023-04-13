<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';
import { Square } from "../models/Square"
import ShowSquare from './ShowSquare.vue';

//Import list:
interface IShowBoardProps {
    players: Player[]
}
const props = defineProps<IShowBoardProps>();

//Variables
let currentPlayer = ref<Player>(props.players[0]);
let squares = ref<Square[]>([
    new Square(""),
    new Square(""),
    new Square(""),
    new Square(""),
    new Square(""),
    new Square(""),
    new Square(""),
    new Square(""),
    new Square(""),
]);

//Play game
function handleClick(i: number) { //hantera once
    squares.value[i].symbol = currentPlayer.value.symbol; //tilldela värde som ska skickas som symbol
    console.log(currentPlayer.value, "clicked square: ", i)

    doWeHaveAWinner(squares.value);

    //toggla spelare
    if (currentPlayer.value.symbol === "X") {
        currentPlayer.value = props.players[1];
    }
    else {
        currentPlayer.value = props.players[0];
    }
    console.log(currentPlayer.value);

}

//Win game:
function doWeHaveAWinner(squares: Square[]) {
    console.log("Squares: ", squares)

};

//Play again:
function playAgain() {
    console.log("You clicked the button 'Play again'!")
}

//Quit game:
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
            <ShowSquare :symbol="squares[index].symbol" @click.once="handleClick(index)" v-for="square, index in squares"
                :key="index" />
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

.index {
    margin: 2px;
}

.index:hover {
    cursor: pointer;
}

.index:active {
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
