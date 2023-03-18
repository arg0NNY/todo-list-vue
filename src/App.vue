<template>
  <AppHeader :subtitle="appSubtitle" />
  <section>
    <div class="container">
      <TaskList :tasks="tasks" />
      <div class="actions">
        <BaseButton primary @click="uploadTasks"><IconCloudUpload />Upload completed tasks</BaseButton>
      </div>
    </div>
  </section>
</template>

<script setup>
import axios from "axios"
import AppHeader from "@/components/parts/AppHeader.vue"
import TaskList from "@/components/task/TaskList.vue"
import {computed} from "vue"
import {useLocalStorage} from "@vueuse/core"
import BaseButton from "@/components/general/BaseButton.vue"
import IconCloudUpload from "@/components/icons/IconCloudUpload.vue"

const tasks = useLocalStorage('tasks', [
  {
    id: 1,
    title: 'The first task',
    desc: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Aliquid illo inventore nisi nobis officiis quae quis quo, repellat ut voluptas?',
    done: false,
    subtasks: [
      {
        title: 'The subtask is here!',
        done: false
      },
      {
        title: 'Meet the second one!',
        done: false
      }
    ]
  }
])

const appSubtitle = computed(() => {
  const activeTasks = tasks.value.filter(t => !t.done)
  return activeTasks.length
    ? `You've got ${activeTasks.length} ${activeTasks.length > 1 ? 'tasks' : 'task'} coming up in the next days.`
    : 'Yay! All the tasks are completed.'
})

function uploadTasks() {
  axios.post('https://some.url/tasks', tasks.value.filter(t => t.done))
}
</script>

<style lang="scss">
@import "assets/css/general";

.actions {
  margin-top: 20px;
  display: flex;
  justify-content: flex-end;
  gap: 10px;
}
</style>
