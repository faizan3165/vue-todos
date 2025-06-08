<script lang="ts" setup>
import type { Task } from "../types";

const { tasks } = defineProps<{
  tasks: Task[];
}>();

const emits = defineEmits<{
  toggleDone: [taskId: string];
  removeTask: [taskId: string];
}>();
</script>

<template>
  <TransitionGroup name="tasks-list" tag="div" class="tasks-list">
    <article v-for="task in tasks" :key="task.id" class="task">
      <label>
        <input
          @input="emits('toggleDone', task.id)"
          type="checkbox"
          :checked="task.done"
        />

        <span :class="{ done: task.done }">
          {{ task?.title }}
        </span>
      </label>

      <button class="outline" @click="emits('removeTask', task.id)">
        Remove
      </button>
    </article>
  </TransitionGroup>
</template>

<style>
.tasks-list {
  margin-top: 1rem;
}

.task {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.done {
  text-decoration: line-through;
}

.tasks-list-enter-active,
.tasks-list-leave-active {
  transition: all 0.3s ease;
}

.tasks-list-enter-from,
.tasks-list-leave-to {
  opacity: 0;
  transform: translateX(300px);
}
</style>
