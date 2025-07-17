<template>
    <div class="calendar">
        <Month />
        <DayCellContainer />
    </div>
</template>

<script setup lang="ts">
import Month from '@/components/Month.vue';
import DayCellContainer from '@/components/DayCellContainer.vue';
</script>

<style scoped>
.calendar {
    display: flex;
    flex-direction: column;
    margin: 0;
    padding: 12px;
    border: 1px solid #eee;
}
</style>
