<script setup>
import SimpleKeyboard from "./components/SimpleKeyboard.vue";
import { onMounted, reactive } from "vue";

const KEY_CODE_MAPPING = {
  13: "{enter}",
  8: "{bksp}"
};

const state = reactive({
  solution: "books",
  guesses: ["", "", "", "", "", ""],
  guessIndex: 0,
})

function handleInput(key) {
  console.log(key);
}

function getKey({ keyCode }) {
  console.log(typeof keyCode, keyCode);
  return  keyCode in KEY_CODE_MAPPING
    ? KEY_CODE_MAPPING[keyCode]
    : String.fromCharCode(keyCode).toLowerCase();
}

onMounted(() => {
  window.addEventListener("keyup", (e) => {
    e.preventDefault();
    let key = getKey(e);
    handleInput(key);
  });
});

</script>

<template>
  <div class="flex flex-col h-screen max-w-md mx-auto justify-evently">
    <simple-keyboard @onKeyPress="handleInput"/>
  </div>
</template>

<style>
</style>
