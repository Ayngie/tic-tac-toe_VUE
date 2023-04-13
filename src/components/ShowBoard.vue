<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';
import { Square } from "../models/Square"
import ShowSquare from './ShowSquare.vue';

//Get list of players:
interface IShowBoardProps {
    players: Player[]
}
const props = defineProps<IShowBoardProps>();

//Variables
let currentPlayer = ref<Player>(props.players[0]);

let squares = ref<Square[]>([
    new Square("", false),
    new Square("", false),
    new Square("", false),
    new Square("", false),
    new Square("", false),
    new Square("", false),
    new Square("", false),
    new Square("", false),
    new Square("", false),
]);


//Play game
function handleClick(i: number) {
    if (squares.value[i].checked === false) {
        squares.value[i].symbol = currentPlayer.value.symbol; //tilldela värde som ska skickas som symbol
        squares.value[i].checked = true;
        console.log(currentPlayer.value.name, "clicked square:", i, " which now has an:", currentPlayer.value.symbol)

        let didThisPlayerWin: boolean = false;
        didThisPlayerWin = doWeHaveAWinner();
        if (didThisPlayerWin === true) {
            console.log(currentPlayer.value.name, "wins!");
        }

        //toggla spelare
        if (currentPlayer.value.symbol === "X") {
            currentPlayer.value = props.players[1];
        }
        else {
            currentPlayer.value = props.players[0];
        }
        console.log("It is now your turn", currentPlayer.value.name);

    }
}

//Win game:
function doWeHaveAWinner() {

    let winningCombos = [[0, 1, 2], [3, 4, 5], [6, 7, 8], [2, 5, 8], [1, 4, 7], [0, 3, 6], [0, 4, 8], [2, 4, 6]];
    let isWinner: boolean = true;

    for (let i = 0; i < winningCombos.length; i++) { // för varje list-objekt i listan med vinnarkombinationer
        let winningComboToCheck = winningCombos[i] // den vinnarkombo i "stora listan" som vi ska kolla
        let squareValuesFromThisGame = squares.value;
        isWinner = true; //måste nollställas innan varje loop

        for (let j = 0; j < winningComboToCheck.length; j++) { //för varje vinnarkombination

            let positionToGetValueFrom = winningComboToCheck[j]; //position på listobjektet (en möljig vinstkombo)
            if (squareValuesFromThisGame[positionToGetValueFrom].symbol !== currentPlayer.value.symbol) { // Om värdet av den positionen på vår lista inte är värdet av nuvarande spelare.
                isWinner = false;
            }
        }
        if (isWinner) return true //returnerar true till vår boolean precis innan anropet av doWeHaveAWinner - dvs variabeln doWeHaveAWinner som var satt till false. Om vi har fått en vinnare här returnas nu true.
    }
    console.log(currentPlayer.value.name, "is Winner =", isWinner);

    return false;

};

// //End game:
// function endGame() {
//     console.log("End game");

// }

//Play again:
function playAgain() {
    for (let i = 0; i < squares.value.length; i++) {
        squares.value[i].symbol = "";
        squares.value[i].checked = false;
    }
    currentPlayer.value = props.players[0];
    console.log("You clicked the button 'Play again'!")
    console.log("It is now your turn", currentPlayer.value.name);
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
            <ShowSquare :symbol="squares[index].symbol" @click="handleClick(index)" v-for="square, index in squares"
                :key="index" />
        </div>
    </div>

    <div class="handle-game-buttons">
        <button type="button" @click="playAgain()"> Play again
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
