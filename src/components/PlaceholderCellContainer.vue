<template>
    <div class="container">
        <div class="cells">
            <PlaceholderCell
                v-for="(cell, i) in cells"
                :key="'cell-' + cell.date"
                :ref="(el) => (cellRefs[i] = el)"
                :date="cell.date"
                :show-content="cell.showContent"
            ></PlaceholderCell>
        </div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import PlaceholderCell from './PlaceholderCell.vue';

const props = defineProps<{
    numPlaceholders: number;
}>();

const BASEINDEX = 31;
const cells = computed(() =>
    Array.from({ length: props.numPlaceholders }, (_, i) => ({
        date: BASEINDEX - i,
        showContent: false,
    })),
);
const cellRefs = ref(<Array<any>>[]);
</script>

<style scoped>
.container {
    display: flex;
    flex-direction: column;
    max-height: fit-content;
}

.cells {
    display: flex;
    flex-wrap: wrap;
    max-width: calc(var(--cell-width) * 7);
    row-gap: 8px;
}
</style>
