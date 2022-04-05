<script setup>
import LetterBox from "./LetterBox.vue";
import { watch, ref, onMounted } from "vue";

const props = defineProps({
    value: String,
    solution: String,
    submitted: Boolean
});

const colors = ref(new Array(props.solution.length).fill(""));

/* onMounted(() => {
    const rows = document.querySelectorAll(".grid");
    console.log("props.solution.length",props.solution.length, `grid-cols-${props.solution.length}`);
    rows.forEach(row => row.classList.add(`grid-cols-${props.solution.length}`))
}); */

watch(
    () => props.submitted,
    async (submitted, prevSubmitted) => {
        if(!props.submitted) return;
        let { solution, value } = props;
        let tempColors = new Array(props.solution.length).fill("gray");
        let letterPool = [];

        for (let index = 0; index < solution.length; index++) {
            console.log(solution.charAt(index) === value.charAt(index), solution.charAt(index), value.charAt(index))
            solution.charAt(index) === value.charAt(index) 
                ? tempColors[index] = "green"
                : letterPool.push(solution.charAt(index));
        }

        for (let index = 0; index < solution.length; index++) {
            let valueChar = value.charAt(index);
            console.log(valueChar, tempColors[index] === "gray", letterPool.indexOf(valueChar) != -1);
            if(tempColors[index] === "gray" && letterPool.indexOf(valueChar) != -1) {
                letterPool.slice(letterPool.indexOf(valueChar), 1);
                tempColors[index] = "yellow";
            }
            colors.value[index] = tempColors[index];
            await new Promise((resolve) => setTimeout(resolve, 300));
        }
    }
);

</script>

<template>
    <div class="grid max-w-xs gap-1 mx-auto mb-1"
    :class="`{ grid-cols-${10} }`">
        <letter-box v-for="index in props.solution.length" :key="index" :letter="value[index-1]" :color="colors[index-1]"/>
    </div>
</template>
