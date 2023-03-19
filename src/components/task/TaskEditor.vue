<template>
  <div class="editor" :class="{ 'editor--done': task.done }">
    <div class="editor__sidebar">
      <Checkbox v-model="task.done" />
    </div>
    <div class="editor__content">
      <TextareaAutoresize class="input modal__title" rows="1" v-model.trim="task.title" placeholder="Add a title..." />
      <TextareaAutoresize class="input modal__desc" rows="2" v-model.trim="task.desc" placeholder="Add a description..." />

      <div class="editor__subtasks">
        <Task v-for="subtask in task.subtasks" :subtask="subtask" />
        <Task subtask create @createTask="createSubtask" />
      </div>

      <div class="modal__actions">
        <BaseButton danger @click="modal = true"><IconTrash />Delete task</BaseButton>
      </div>
    </div>
  </div>

  <Teleport to="body">
    <Transition name="modal">
      <Modal submodal common v-if="modal" @close="modal = false">
        <template #title>Deletion confirmation</template>
        Are you sure you want to delete <b>{{ task.title }}</b> task? This action cannot be undone.
        <template #actions>
          <BaseButton danger @click="emit('remove')"><IconTrash />Delete</BaseButton>
          <BaseButton @click="modal = false">Cancel</BaseButton>
        </template>
      </Modal>
    </Transition>
  </Teleport>
</template>

<script setup>
import Checkbox from "@/components/general/Checkbox.vue"
import {onBeforeMount, ref} from "vue"
import Task from "@/components/task/Task.vue"
import BaseButton from "@/components/general/BaseButton.vue"
import IconTrash from "@/components/icons/IconTrash.vue"
import TextareaAutoresize from "@/components/general/TextareaAutoresize.vue"
import Modal from "@/components/general/Modal.vue"

const props = defineProps({
  task: {
    type: Object,
    required: true
  }
})
const emit = defineEmits(['remove'])

const modal = ref(false)

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
  &__subtasks {
    margin-top: 30px;
    display: flex;
    flex-direction: column;
    gap: 15px;
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
    &__subtasks {
      margin-top: 15px;
      gap: 10px;
    }
  }
}
</style>
