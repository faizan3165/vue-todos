<script lang="ts" setup>
import { computed, ref } from "vue";

import TaskForm from "./components/TaskForm.vue";
import TaskList from "./components/TaskList.vue";
import FilterButton from "./components/FilterButton.vue";

import type { Task } from "./types";

const message = ref("Tasks App");
const tasks = ref<Task[]>([]);
const filter = ref<string>("all");
const currentFilter = ref<string>("all");

const completedTasks = computed(() =>
  tasks.value.reduce((total, tasks) => (tasks.done ? total + 1 : total), 0)
);

function addTask(newTask: string) {
  tasks.value.push({
    id: crypto.randomUUID(),
    title: newTask,
    done: false,
  });
}

function toggleDone(taskId: string) {
  const task = tasks.value.find((task) => task.id === taskId);
  if (task) {
    task.done = !task.done;
  }
}

function removeTask(taskId: string) {
  tasks.value = tasks.value.filter((task) => task.id !== taskId);
}

function setFilter(newFilter: string) {
  filter.value = newFilter;
  currentFilter.value = newFilter;
}

const filteredTasks = computed(() => {
  switch (filter.value) {
    case "all":
      return tasks.value;
      break;

    case "todo":
      return tasks.value.filter((task) => !task.done);
      break;

    case "done":
      return tasks.value.filter((task) => task.done);
      break;

    default:
      return tasks.value;
      break;
  }
});
</script>

<template>
  <main>
    <h1>{{ message }}</h1>

    <TaskForm @add-task="addTask" />

    <h3 v-if="!tasks.length">Add a task to get started</h3>
    <h3 v-else>{{ completedTasks }} / {{ tasks?.length }} Tasks Completed</h3>

    <div class="button-container" v-if="tasks.length">
      <FilterButton
        v-for="f in ['all', 'todo', 'done']"
        :key="f"
        :filter="f"
        :currentFilter="currentFilter"
        @set-filter="setFilter"
      />
    </div>

    <TaskList
      :tasks="filteredTasks"
      @toggle-done="toggleDone"
      @remove-task="removeTask"
    />
  </main>
</template>

<style>
main {
  max-width: 800px;
  margin: 1rem auto;
  padding: 2rem;
}

.button-container {
  display: flex;
  justify-content: flex-end;
  gap: 0.5rem;
}
</style>
