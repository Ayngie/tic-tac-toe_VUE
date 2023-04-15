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
let ongoingGame = false;
let playerList = ref<Player[]>(props.players);
let currentPlayer = ref<Player>(props.players[0]);
let itsATie = false;
let aPlayerHasWon = false;
let winnerWas = ref<Player>({ name: "", symbol: "", score: 0 });
let weHaveAScore = false;
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

// Retrieve ongoing game players list from localStorage:
let storedPlayersList: Player[] = JSON.parse(
    localStorage.getItem("players") || "[]"
);
// console.log(playerList)
if (storedPlayersList.length > 0) {
    playerList.value = storedPlayersList;
    // console.log("Updated playerList is:", playerList.value);
    ongoingGame = true;
}

//Retrieve ongoing game squares list from localStorage:
let storedSquaresList: Square[] = JSON.parse(
    localStorage.getItem("storedSquares") || "[]"
);
if (storedSquaresList.length > 0) {
    squares.value = storedSquaresList;
}

//Retrieve ongoing game currentPlayer from localStorage:
if (ongoingGame === true) {
    currentPlayer.value = JSON.parse(
        localStorage.getItem("currentPlayer") || "");
}

//Retrieve ongoing game winner from localStorage:
let storedAPlayerHasWon: boolean = JSON.parse(
    localStorage.getItem("storeThatAPlayerHasWon") || "");
if (storedAPlayerHasWon === true) {
    aPlayerHasWon = true;
}

// Maintain ongoing game winner header
let storedWinner: Player = JSON.parse(
    localStorage.getItem("storeWinner") || ""
);
if (storedWinner.name !== "") {
    winnerWas.value = storedWinner;
}


//Play game
function handleClick(i: number) {
    if (!aPlayerHasWon) {
        if (squares.value[i].checked === false) {
            squares.value[i].symbol = currentPlayer.value.symbol; //tilldela värde som ska skickas som symbol
            squares.value[i].checked = true;
            // console.log(currentPlayer.value.name, "clicked square:", i, " which now has an:", currentPlayer.value.symbol)
            localStorage.setItem("storedSquares", JSON.stringify(squares.value)); //save to local storage

            //did somebody win?
            let didThisPlayerWin: boolean = false;
            didThisPlayerWin = doWeHaveAWinner(); //sätter didThisPlayerWin till true vid vinst
            if (didThisPlayerWin === true) {
                // console.log(currentPlayer.value.name, "wins!");
                aPlayerHasWon = true;
                localStorage.setItem("storeThatAPlayerHasWon", JSON.stringify(aPlayerHasWon)); //save to local storage
                winnerWas.value = currentPlayer.value;
                localStorage.setItem("storeWinner", JSON.stringify(winnerWas.value)); //save to local storage
                currentPlayer.value.score++; //increase winners score
                // console.log("Score is:", props.players[0].name, ":", props.players[0].score, "vs.", props.players[1].name, ":", props.players[1].score);
                localStorage.setItem("players", JSON.stringify(props.players)); //save to local storage
                weHaveAScore = true;
            }

            //is it a tie?
            let allBoxesChecked: boolean = false;
            allBoxesChecked = doWeHaveATie(); //sätter isItATie till true vid oavgjort
            if (allBoxesChecked === true) {
                // console.log("All boxes are checked.");
                if (!aPlayerHasWon) {
                    // console.log("Oops, it's a tie...");
                    itsATie = true;
                }
            }

            //toggla spelare
            if (currentPlayer.value.symbol === "X") {
                currentPlayer.value = props.players[1]; // VÄNTA NU, STÄMMER DETTA? Det är väl spelare O?
            }
            else {
                currentPlayer.value = props.players[0];
            }
            // console.log("It is now your turn", currentPlayer.value.name);

            if (ongoingGame) {
                localStorage.setItem("currentPlayer", JSON.stringify(currentPlayer.value)); //save to local storage
            }
        }
    }
}

//Tie game:
function doWeHaveATie() {
    let areAllBoxesChecked = ref<number>(0);
    for (let i = 0; i < squares.value.length; i++) {
        if (squares.value[i].checked) {
            areAllBoxesChecked.value++;
        }
    }
    // console.log("Nr of boxes checked:", areAllBoxesChecked.value)
    if (areAllBoxesChecked.value === 9) {
        return true; //returnerar true till vår boolean allBoxesChecked
    }
    return false; //return false (så ej blir true vs void)
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
    // console.log(currentPlayer.value.name, "is Winner =", isWinner);

    return false;
};

//Play again:
function playAgain() {
    for (let i = 0; i < squares.value.length; i++) {
        squares.value[i].symbol = "";
        squares.value[i].checked = false;
    }
    itsATie = false;
    aPlayerHasWon = false;
    winnerWas.value = ({ name: "", symbol: "", score: 0 });
    ongoingGame = false
    // console.log("You clicked the button 'Play again'!")
    // console.log("It is now your turn", currentPlayer.value.name);
}

//Quit game:
let emit = defineEmits(["quitGame"])
function quitGame() {
    // console.log("You clicked the button 'Quit game'!")
    emit("quitGame")
}

</script>

<template>
    <h1> Tic-tac-toe - {{ props.players[0].name }} vs. {{ props.players[1].name }}</h1>
    <div v-if="!itsATie">
        <h2 v-if="!aPlayerHasWon">{{ currentPlayer.name }} - make your move ({{ currentPlayer.symbol }})!</h2>
        <h2 v-else-if="aPlayerHasWon || ongoingGame" class="blink_me">Congrats {{ winnerWas.name }} - you won!</h2>
        <p v-if="weHaveAScore || ongoingGame">Score is: {{ props.players[0].name }}: {{ props.players[0].score }} vs. {{
            props.players[1].name }}:
            {{ props.players[1].score }};
        </p>
    </div>
    <div v-else-if="itsATie">
        <h2>Ooops... It's a tie. Play again?</h2>
    </div>
    <div class="container">
        <div class="board">
            <ShowSquare :symbol="squares[index].symbol" @click="handleClick(index)" v-for="square, index in squares"
                :key="index" :class="squares[index].symbol === 'X' ? 'X' : 'O'" />
        </div>
    </div>

    <div class="handle-game-buttons">
        <button type="button" class="button" @click="playAgain()"> Play again
        </button>
        <button type="button" class="button" @click.once="quitGame()"> Quit
            game
        </button>
    </div>
</template>

<style scoped>
h1 {
    color: green;
}

.green {
    color: green;
}

.blink_me {
    color: gold;
    animation: blinker 1s linear infinite;
}

@keyframes blinker {
    50% {
        opacity: 0.5;
    }
}

.container {
    display: flex;
    justify-content: center;
    align-items: center;
}

.board {
    display: flex;
    flex-wrap: wrap;
    width: 320px;
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

.X {
    color: purple;
}

.O {
    color: salmon;
}

.handle-game-buttons {
    padding: 15px;
}

.button {
    margin: 10px;
    background-color: rgb(221, 255, 221);
}

.button:hover {
    border-color: green;
}

.button:focus {
    outline: 2px solid green;
}
</style>
