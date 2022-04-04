<script setup>
import SimpleKeyboard from "./components/SimpleKeyboard.vue";
import WordRow from "./components/WordRow.vue";
import LetterBox from "./components/LetterBox.vue";
import { onMounted, reactive } from "vue";

const KEY_CODE_MAPPING = {
    13: "{enter}",
    8: "{bksp}",
};

const WINNING_WORD = "palabras";

const state = reactive({
    solution: WINNING_WORD,
    guesses: new Array(WINNING_WORD.length).fill(""),
    guessIndex: 0,
    usedLetters: {
        miss: [],
        found: [],
        hint: [],
    },
});

function isValidKey(key) {
    const alphaRegex = /^[a-zA-Z]+$/;
    return alphaRegex.test(key);
}

const isInSolution = (c) => state.solution.indexOf(c) != -1;

function handleInput(key) {
    if (state.guessIndex >= 6) return;

    const currentGuess = state.guesses[state.guessIndex];

    if (key === "{enter}" && currentGuess.length === state.solution.length) {
        state.guessIndex++;
        for (let index in currentGuess) {
            let c = currentGuess.charAt(index);
            if (c === state.solution.charAt(index)) state.usedLetters.found.push(c);
            if (isInSolution(c)) state.usedLetters.hint.push(c);
            if (!isInSolution(c)) state.usedLetters.miss.push(c);
        }
    }
    if (key === "{bksp}") state.guesses[state.guessIndex] = currentGuess.slice(0, -1);
    if (currentGuess.length < state.solution.length && isValidKey(key)) state.guesses[state.guessIndex] += key;
}

function getKey({ keyCode }) {
    return keyCode in KEY_CODE_MAPPING ? KEY_CODE_MAPPING[keyCode] : String.fromCharCode(keyCode).toLowerCase();
}

onMounted(() => {
    window.addEventListener("keyup", (e) => {
        e.preventDefault();
        let key = getKey(e);
        console.log(e.keyCode);
        handleInput(key);
    });
});
</script>

<template>
    <div class="flex flex-col h-screen max-w-md mx-auto justify-evently">
        <div>
            <word-row
                v-for="(guess, index) in state.guesses"
                :key="index"
                :value="guess"
                :solution="state.solution"
                :submitted="index < state.guesses"
            />
        </div>
        <simple-keyboard @onKeyPress="handleInput" :usedLetters="state.usedLetters" />
    </div>
</template>

<style></style>
