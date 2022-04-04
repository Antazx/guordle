<script setup>
import LetterBox from "./LetterBox.vue";
import { watch, ref, onMounted } from "vue";

const props = defineProps({
    value: String,
    solution: String,
    submitted: Boolean
});

const colores = ref(new Array(5).fill("gray"));


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
        let tempColors = new Array(5).fill("gray");
    }
);

</script>

<template>
    <div class="grid max-w-xs gap-1 mx-auto mb-1" :class="[`grid-cols-${props.solution.length}`]">
        <letter-box v-for="index in props.solution.length" :key="index" :letter="value[index-1]"/>
    </div>
</template>
