<template>
    <div class="cell" :style="cellStyle" @dragstart.prevent>
        <div v-if="showContent" class="content">{{ date }}</div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

const props = defineProps<{
    date: number;
    showContent: boolean;
}>();

const cellStyle = computed(() => ({
    backgroundColor: 'transparent',
}));

const showContent = ref(props.showContent);
</script>

<style scoped>
.cell {
    width: var(--cell-width);
    height: var(--cell-height);
    user-select: none;
    cursor: pointer;
    display: flex;
    align-items: center;
    align-content: center;
    justify-content: center;
}

.content {
    font-weight: 600;
    font-size: 16pt;
}
</style>
