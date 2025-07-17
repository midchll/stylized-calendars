<template>
    <div class="container">
        <div v-if="showControls" class="control">
            <div class="month-incr" @click.stop="incrMonth(true)">&and;</div>
            <div class="month-incr" @click.stop="incrMonth(false)">&or;</div>
        </div>
        <h1 ref="monthLabelRef" class="label" @click.stop="showControls = !showControls">
            {{ month }}
        </h1>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';

const months = [
    'January',
    'February',
    'March',
    'April',
    'May',
    'June',
    'July',
    'August',
    'September',
    'October',
    'November',
    'December',
];
const showControls = ref(false);
const monthIndex = ref(0);
const month = ref(months[monthIndex.value]);
const monthLabelRef = ref<HTMLElement | null>(null);

function incrMonth(dir: boolean) {
    if (dir) {
        monthIndex.value < months.length - 1 ? monthIndex.value++ : (monthIndex.value = 0);
    } else {
        monthIndex.value > 0 ? monthIndex.value-- : (monthIndex.value = months.length - 1);
    }
    month.value = months[monthIndex.value];
}
</script>

<style scoped>
.container {
    display: flex;
    height: 36px;
    width: fit-content;
    align-items: center;
    gap: 2px;
}

.label {
    font-size: 16pt;
    padding: 4px 8px;
    width: fit-content;
    margin: 0;
    color: #000;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.2s ease;
}

.label:hover {
    background-color: #eee;
}

.control {
    display: flex;
    flex-direction: column;
    gap: 2px;
}

.month-incr {
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    width: 16px;
    height: 16px;
    border-radius: 6px;
    background-color: #ddd;
    transition: all 0.2s ease;
}

.month-incr:hover {
    background-color: #ccc;
}
</style>
