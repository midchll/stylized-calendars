<template>
    <div
        class="cell"
        :style="cellStyle"
        @dragstart.prevent
        @mouseenter="isHovered = true"
        @mouseleave="isHovered = false"
    >
        <div v-if="endpoint || props.containerHovered" class="content">{{ date + 1 }}</div>
    </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';

const props = defineProps<{
    date: number;
    selected: boolean;
    endpoint: boolean;
    containerHovered: boolean;
}>();

const topRightRadius = ref('0');
const topLeftRadius = ref('0');
const bottomRightRadius = ref('0');
const bottomLeftRadius = ref('0');
const isHovered = ref(false);
const containerHovered = ref(props.containerHovered);
const fill = computed(() => (props.selected ? '#FFE500' : '#FFFFFF'));
const cellStyle = computed(() => ({
    backgroundColor:
        isHovered.value && props.selected
            ? '#EED455'
            : isHovered.value && !props.selected
              ? '#eee'
              : fill.value,
    borderTopRightRadius: topRightRadius.value,
    borderTopLeftRadius: topLeftRadius.value,
    borderBottomRightRadius: bottomRightRadius.value,
    borderBottomLeftRadius: bottomLeftRadius.value,
}));

function setBorderRadius(tl: number, tr: number, br: number, bl: number) {
    const px = (v: number) => `${v}px`;
    topLeftRadius.value = px(tl);
    topRightRadius.value = px(tr);
    bottomRightRadius.value = px(br);
    bottomLeftRadius.value = px(bl);
}

function peekContent(toggle: boolean) {
    containerHovered.value = toggle;
}
defineExpose({ setBorderRadius });
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
