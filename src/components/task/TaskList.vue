<template>
  <TransitionGroup tag="div" name="task-list" class="task-list">
    <Task create @createTask="addTask" key="create" />
    <Task v-for="task in tasks.slice().reverse()" :key="task.id" :task="task" @remove="removeTask" />
  </TransitionGroup>
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
