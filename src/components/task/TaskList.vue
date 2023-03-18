<template>
  <div class="task-list">
    <Task create @createTask="addTask" />
    <Task v-for="task in tasks.slice().reverse()" :key="task.id" :task="task" @remove="removeTask" />
  </div>
</template>

<script setup>
import Task from "@/components/task/Task.vue"

const props = defineProps({
  tasks: {
    type: Array,
    required: true
  }
})

function addTask(task) {
  task.id = props.tasks[props.tasks.length - 1].id + 1
  props.tasks.push(task)
}

function removeTask(id) {
  props.tasks.splice(props.tasks.findIndex(t => t.id === id), 1)
}
</script>

<style scoped lang="scss">
@import "@/assets/css/_includes.scss";

.task-list {
  display: flex;
  flex-direction: column;
  gap: 10px;

  @include mobile {
    gap: 5px;
  }
}
</style>
