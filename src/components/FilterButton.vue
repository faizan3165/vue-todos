<script lang="ts" setup>
const { filter, currentFilter } = defineProps<{
  filter: string;
  currentFilter: string;
}>();

const emits = defineEmits<{
  setFilter: [filter: string];
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
