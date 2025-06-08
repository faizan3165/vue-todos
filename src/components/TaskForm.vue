<script lang="ts" setup>
import { ref } from "vue";

const emit = defineEmits<{
  addTask: [newTask: string];
}>();

const newTask = ref("");
const error = ref("");

function handleSubmit() {
  if (newTask?.value?.trim()) {
    emit("addTask", newTask.value?.trim());
    newTask.value = "";
  } else {
    error.value = "Task cannot be empty";
  }
}
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <label for="newTask">New Task</label>

    <input
      type="text"
      name="newTask"
      id="newTask"
      v-model="newTask"
      :aria-invalid="!!error || undefined"
      @input="error = ''"
    />

    <small id="invalid-helper" v-if="error">
      {{ error }}
    </small>

    <div class="button-container">
      <button>Add</button>
    </div>
  </form>
</template>
