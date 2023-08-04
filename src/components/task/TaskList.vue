<template>
  <TransitionGroup tag="div" name="task-list" class="task-list">
    <Task create @createTask="addTask" key="create" />
    <Task
      v-for="id in tasks.slice().reverse().map(t => t.id)"
      :key="id"
      v-model:task="tasks[tasks.findIndex(t => t.id === id)]"
      @remove="removeTask"
    />
  </TransitionGroup>
</template>

<script setup>
import Task from "@/components/task/Task.vue"
import { computed } from 'vue'

const props = defineProps({
  tasks: {
    type: Array,
    required: true
  }
})
const emit = defineEmits(['update:tasks'])

const tasks = computed({
  get: () => props.tasks,
  set: value => emit('update:tasks', value)
})

function addTask(task) {
  task.id = tasks.value[tasks.value.length - 1].id + 1
  tasks.value.push(task)
}

function removeTask(id) {
  tasks.value.splice(tasks.value.findIndex(t => t.id === id), 1)
}
</script>

<style scoped lang="scss">
@import "@/assets/css/_includes.scss";

.task-list {
  position: relative;

  & > *:not(:last-child) {
    margin-bottom: 10px;

    @include mobile {
      margin-bottom: 5px;
    }
  }
}

.task-list-enter-active, .task-list-leave-active, .task-list-move {
  transition: .3s all;
}
.task-list-leave-active {
  position: absolute;
}
.task-list-enter-from {
  transform: translateY(-70px);
  @include mobile {
    transform: translateY(-45px);
  }
}
.task-list-leave-to {
  opacity: 0;
}
</style>
