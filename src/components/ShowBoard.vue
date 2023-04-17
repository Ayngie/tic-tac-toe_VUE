<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';
import { Square } from "../models/Square"
import ShowSquare from './ShowSquare.vue';

//Get list of players:
interface IShowBoardProps {
    players: Player[]
    gameOn: boolean
}
const props = defineProps<IShowBoardProps>();

//Variables
let currentPlayer = ref<Player>(props.players[0]);
let itsATie = ref(false);
let aPlayerHasWon = false;
let winnerWas = ref<Player>({ name: "", symbol: "", score: 0 });
let weHaveAScore = ref(false);
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
let ongoingGame = ref(props.gameOn); //blir true om spel är igång!

// Retrieve from localStorage: 
// // squares list
let storedSquaresList: Square[] = JSON.parse(
    localStorage.getItem("storedSquares") || "[]"
);
if (storedSquaresList.length > 0) {
    squares.value = storedSquaresList;
}

//Play game
function handleClick(i: number) {
    if (!aPlayerHasWon) {
        if (squares.value[i].checked === false) {
            squares.value[i].symbol = currentPlayer.value.symbol; //tilldela värde som ska skickas som symbol
            squares.value[i].checked = true;
            console.log(currentPlayer.value.name, "clicked square:", i, " which now has an:", currentPlayer.value.symbol)
            localStorage.setItem("storedSquares", JSON.stringify(squares.value));

            //did somebody win?
            let didThisPlayerWin: boolean = false;
            didThisPlayerWin = doWeHaveAWinner(); //sätter didThisPlayerWin till true vid vinst
            if (didThisPlayerWin === true) {
                console.log(currentPlayer.value.name, "wins!");
                aPlayerHasWon = true;

                //winner has won, mark all squares as checked
                for (let i = 0; i < squares.value.length; i++) {
                    squares.value[i].checked = true;
                }
                localStorage.setItem("storedSquares", JSON.stringify(squares.value));

                winnerWas.value = currentPlayer.value;
                localStorage.setItem("storeWinner", JSON.stringify(winnerWas.value));

                currentPlayer.value.score++; //increase winners score
                console.log("Score is:", props.players[0].name, ":", props.players[0].score, "vs.", props.players[1].name, ":", props.players[1].score);
                localStorage.setItem("storedPlayers", JSON.stringify(props.players));

                weHaveAScore.value = true;
            }

            //is it a tie?
            let allBoxesChecked: boolean = false;
            allBoxesChecked = doWeHaveATie(); //sätter isItATie till true vid oavgjort
            if (allBoxesChecked === true) {
                console.log("All boxes are checked.");
                if (!aPlayerHasWon) {
                    console.log("Oops, it's a tie...");
                    itsATie.value = true;
                }
            }

            //toggla spelare
            if (currentPlayer.value.symbol === "X") {
                currentPlayer.value = props.players[1];
            }
            else {
                currentPlayer.value = props.players[0];
            }
            console.log("It is now your turn", currentPlayer.value.name);

            // if (ongoingGame) localStorage.setItem("storedCurrentPlayer", JSON.stringify(currentPlayer.value));
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
    console.log("Nr of boxes checked:", areAllBoxesChecked.value)
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
    console.log(currentPlayer.value.name, "is Winner =", isWinner);

    return false;
};

//Play again:
function playAgain() {
    localStorage.setItem("storedSquares", JSON.stringify(squares.value)); //save to local storage

    for (let i = 0; i < squares.value.length; i++) {
        squares.value[i].symbol = "";
        squares.value[i].checked = false;
    }
    itsATie.value = false;
    aPlayerHasWon = false;
    winnerWas.value = ({ name: "", symbol: "", score: 0 });
    ongoingGame.value = false
    console.log("You clicked the button 'Play again'!")
    console.log("It is now your turn", currentPlayer.value.name);
}

//Quit game:
let emit = defineEmits(["quitGame"])
function quitGame() {
    localStorage.clear();
    console.log("You clicked the button 'Quit game'!")
    emit("quitGame")
}
</script>

<template>
    <h1> Tic-tac-toe - {{ props.players[0].name }} vs. {{ props.players[1].name }}</h1>
    <div v-if="!itsATie">
        <h2 v-if="!aPlayerHasWon">{{ currentPlayer.name }} - make your move ({{ currentPlayer.symbol }})!</h2>
        <h2 v-else-if="aPlayerHasWon" class="blink_me">Congrats {{ winnerWas.name }} - you won!</h2>
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
