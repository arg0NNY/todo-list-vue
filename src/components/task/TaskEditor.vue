<template>
  <div class="editor" :class="{ 'editor--done': task.done }">
    <div class="editor__sidebar">
      <Checkbox v-model="task.done" />
    </div>
    <div class="editor__content">
      <input type="text" class="input editor__title" v-model.trim="task.title" placeholder="Add a title...">
      <textarea ref="textarea" class="input editor__desc" @input="resizeTextarea" v-model.trim="task.desc" placeholder="Add a description..."></textarea>

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
import {onBeforeMount, onMounted, ref} from "vue"
import Task from "@/components/task/Task.vue"
import BaseButton from "@/components/general/BaseButton.vue"
import IconTrash from "@/components/icons/IconTrash.vue";

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

const textarea = ref()
function resizeTextarea() {
  if (!textarea.value) return
  textarea.value.style.height = "40px"
  textarea.value.style.height = textarea.value.scrollHeight + "px"
}

onBeforeMount(() => {
  props.task.desc = props.task.desc ?? ''
})

onMounted(() => resizeTextarea())
</script>

<style scoped lang="scss">
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
    resize: none;
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
}
</style>
