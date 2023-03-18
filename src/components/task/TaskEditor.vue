<template>
  <div class="editor" :class="{ 'editor--done': task.done }">
    <div class="editor__sidebar">
      <Checkbox v-model="task.done" />
    </div>
    <div class="editor__content">
      <TextareaAutoresize class="input editor__title" rows="1" v-model.trim="task.title" placeholder="Add a title..." />
      <TextareaAutoresize class="input editor__desc" rows="2" v-model.trim="task.desc" placeholder="Add a description..." />

      <div class="editor__subtasks">
        <Task v-for="subtask in task.subtasks" :subtask="subtask" />
        <Task subtask create @createTask="createSubtask" />
      </div>

      <div class="editor__actions">
        <BaseButton danger @click="emit('remove')"><IconTrash />Remove task</BaseButton>
      </div>
    </div>
  </div>
</template>

<script setup>
import Checkbox from "@/components/general/Checkbox.vue"
import {onBeforeMount} from "vue"
import Task from "@/components/task/Task.vue"
import BaseButton from "@/components/general/BaseButton.vue"
import IconTrash from "@/components/icons/IconTrash.vue"
import TextareaAutoresize from "@/components/general/TextareaAutoresize.vue"

const props = defineProps({
  task: {
    type: Object,
    required: true
  }
})
const emit = defineEmits(['remove'])

function createSubtask(task) {
  props.task.subtasks.push({
    title: task.title,
    done: false
  })
}

onBeforeMount(() => {
  props.task.desc = props.task.desc ?? ''
})
</script>

<style scoped lang="scss">
@import "@/assets/css/_includes.scss";

.editor {
  display: flex;
  gap: 20px;
  align-items: flex-start;

  &__sidebar {
    padding-top: 4px;
  }
  &__content {
    flex: 1;
  }
  &__title {
    font-size: 24px;
    font-weight: 700;
  }
  &__desc {
    margin-top: 10px;
  }
  &__subtasks {
    margin-top: 30px;
    display: flex;
    flex-direction: column;
    gap: 15px;
  }
  &__actions {
    margin-top: 20px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    gap: 10px;
  }

  &--done {
    .editor__title {
      text-decoration: line-through;
    }
  }

  @include mobile {
    gap: 10px;

    &__sidebar {
      padding-top: 1px;
    }
    &__title {
      font-size: 18px;
    }
    &__desc {
      margin-top: 7px;
    }
    &__subtasks {
      margin-top: 15px;
      gap: 10px;
    }
    &__actions {
      margin-top: 15px;
      gap: 5px;
    }
  }
}
</style>
