<script lang="ts" setup>
import { TaskFilter } from "../types";

const { filter, currentFilter } = defineProps<{
  filter: TaskFilter;
  currentFilter: TaskFilter;
}>();

const emits = defineEmits<{
  setFilter: [filter: TaskFilter];
}>();
</script>

<template>
  <button
    class="secondary"
    :class="{ contrast: filter === currentFilter }"
    @click="emits('setFilter', filter)"
  >
    {{ filter }}
  </button>
</template>

<style scoped>
button {
  text-transform: capitalize;
}
</style>
