<template>
    <div class="containers">
        <div class="day-labels" @click.stop="showPlaceholderControl = !showPlaceholderControl">
            <h2 class="dotw">S</h2>
            <h2 class="dotw">M</h2>
            <h2 class="dotw">T</h2>
            <h2 class="dotw">W</h2>
            <h2 class="dotw">T</h2>
            <h2 class="dotw">F</h2>
            <h2 class="dotw">S</h2>
        </div>
        <div v-if="showPlaceholderControl" class="placeholder-control">
            <div class="placeholder-incr" @click.stop="incrPlaceholder(false)">&lt;</div>
            <div class="placeholder-incr" @click.stop="incrPlaceholder(true)">&gt;</div>
        </div>
        <div
            class="cells"
            @mouseenter="containerHovered = true"
            @mouseleave="containerHovered = false"
        >
            <PlaceholderCellContainer :numPlaceholders="numPlaceholders"></PlaceholderCellContainer>
            <DayCell
                v-for="(cell, i) in cells"
                :key="'cell-' + cell.date"
                :ref="(el) => (cellRefs[i] = el)"
                :date="cell.date"
                :selected="cell.selected"
                :endpoint="cell.endpoint"
                :containerHovered="containerHovered"
                @mousedown="
                    clearPrev();
                    dragEndIndex = -1;
                    dragStartIndex = cell.date;
                    cell.endpoint = true;
                "
                @mouseup="
                    dragEndIndex = cell.date;
                    cell.endpoint = true;
                    updateSelection();
                "
            ></DayCell>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import DayCell from './DayCell.vue';
import PlaceholderCellContainer from './PlaceholderCellContainer.vue';

const NUM_DAYS = 31;
const showPlaceholderControl = ref(false);
const numPlaceholders = ref(0);
const containerHovered = ref(false);
const cells = ref(
    Array.from({ length: NUM_DAYS }, (_, i) => ({
        date: i,
        selected: false,
        endpoint: false,
    })),
);
const cellRefs = ref<any[]>([]);
const dragStartIndex = ref(-1);
const dragEndIndex = ref(-1);

function updateSelection() {
    if (dragStartIndex.value === -1 || dragEndIndex.value === -1) return;
    const start = Math.min(dragStartIndex.value, dragEndIndex.value);
    const end = Math.max(dragStartIndex.value, dragEndIndex.value);

    // reset all
    for (let i = 0; i < cells.value.length; i++) {
        const cell = cells.value[i];
        const ref = cellRefs.value[i];
        cell.selected = false;
        cell.endpoint = false;

        if (!ref?.setBorderRadius) {
            ref.setBorderRadius(0, 0, 0, 0);
        }
    }

    for (let i = start; i <= end; i++) {
        cells.value[i].selected = true;
    }

    cells.value[start].endpoint = true;
    cells.value[end].endpoint = true;

    for (let i = start; i <= end; i++) {
        const visualIndex = (i + numPlaceholders.value) % 7;
        const isRowStart = visualIndex === 0;
        const isRowEnd = visualIndex === 6;
        const isStart = i === start;
        const isEnd = i === end;
        const isSingle = isStart && isEnd;
        const ref = cellRefs.value[i];

        if (!ref?.setBorderRadius) continue;

        let tl = 0,
            tr = 0,
            br = 0,
            bl = 0;

        if (isSingle) {
            tl = tr = br = bl = 20;
        } else {
            if (isStart || isRowStart) {
                tl = bl = 20;
            }
            if (isEnd || isRowEnd) {
                tr = br = 20;
            }
        }
        ref.setBorderRadius(tl, tr, br, bl);
    }
}

function incrPlaceholder(dir: boolean) {
    if (dir) {
        if (numPlaceholders.value < 6) numPlaceholders.value++;
    } else if (!dir) {
        if (numPlaceholders.value > 0) numPlaceholders.value--;
    }
    updateSelection();
}

function clearPrev() {
    for (let i = 0; i < cells.value.length; i++) {
        cells.value[i].selected = false;
        cells.value[i].endpoint = false;
    }
}

function peekContent(toggle: boolean) {
    if (toggle) {
        for (let i = 0; i < cellRefs.value.length; i++) {
            cellRefs.value[i].peekContent(true);
        }
    }
    if (!toggle) {
        for (let i = 0; i < cellRefs.value.length; i++) {
            cellRefs.value[i].peekContent(false);
        }
    }
}
</script>

<style scoped>
.placeholder-control {
    display: flex;
    justify-content: center;
    gap: 2px;
    width: 100%;
}

.placeholder-incr {
    display: flex;
    justify-content: center;
    align-items: center;
    text-align: center;
    width: calc(var(--cell-height) / 2);
    height: calc(var(--cell-height) / 2 - 1);
    border-radius: 6px;
    background-color: #eee;
    transition: all 0.2s ease;
    user-select: none;
    cursor: pointer;
}

.placeholder-incr:hover {
    background-color: #ddd;
}

.container {
    display: flex;
    flex-direction: column;
    max-height: fit-content;
    width: fit-content;
}

.cells {
    display: flex;
    flex-wrap: wrap;
    max-width: calc(var(--cell-width) * 7);
    row-gap: 8px;
}

.day-labels {
    display: flex;
    height: fit-content;
    cursor: pointer;
    align-items: center;
    align-content: center;
    border-radius: 40px;
}

.day-labels:hover {
    background-color: #eee;
}

.day-labels h2 {
    width: var(--cell-width);
    height: var(--cell-height);
    font-size: 16pt;
    text-align: center;
    align-content: center;
}
</style>
