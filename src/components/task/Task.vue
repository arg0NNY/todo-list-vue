<template>
  <div class="task" @click="modal = true" :class="{ 'task--subtask': subtask, 'task--done': !create && (task?.done || subtask?.done), 'task--create': create, 'task--creating': create && focused }">

    <template v-if="create || subtask">
      <div class="task__sidebar">
        <Checkbox v-if="create" />
        <Checkbox v-else v-model="subtask.done" />
      </div>
      <div class="task__content">
        <form @submit.prevent="createTask">
          <TextareaAutoresize v-if="create" ref="input" rows="1" v-model.trim="inputValue" type="text" class="input task__title" :placeholder="`Add a new ${subtask ? 'subtask' : 'task'}...`" />
          <TextareaAutoresize v-else ref="input" rows="1" v-model.trim="subtask.title" type="text" class="input task__title" :placeholder="`Add a title...`" />
        </form>
      </div>
    </template>

    <template v-else>
      <div class="task__sidebar">
        <Checkbox v-model="task.done" />
      </div>
      <div class="task__content">
        <h3 class="task__title">{{ task.title }}</h3>
        <div v-if="task.desc" class="task__desc">{{ task.desc }}</div>
        <div class="task__bottom" v-if="task.subtasks.length">
          <Chip><IconList/><span>{{ task.subtasks.filter(t => t.done).length }}/{{ task.subtasks.length }} subtasks</span></Chip>
        </div>
      </div>

      <Teleport to="body">
        <Transition name="modal">
          <Modal v-if="modal" @close="modal = false">
            <TaskEditor :task="task" @remove="emit('remove', task.id)" />
          </Modal>
        </Transition>
      </Teleport>
    </template>

  </div>
</template>

<script setup>
import Checkbox from "@/components/general/Checkbox.vue"
import Chip from "@/components/general/Chip.vue"
import IconList from "@/components/icons/IconList.vue"
import {useFocus} from "@vueuse/core"
import {ref, watch} from "vue"
import Modal from "@/components/general/Modal.vue"
import TaskEditor from "@/components/task/TaskEditor.vue"
import TextareaAutoresize from "@/components/general/TextareaAutoresize.vue";

const props = defineProps({
  task: Object,
  subtask: [Object, Boolean],
  create: {
    type: Boolean,
    default: false
  }
})
const emit = defineEmits(['createTask', 'remove'])

const input = ref()
const inputValue = ref('')
const { focused } = useFocus(input)

watch(focused, v => !v && inputValue.value.trim() !== '' && createTask())

const modal = ref(false)

function createTask() {
  if (!props.create || inputValue.value.trim() === '') return

  emit('createTask', {
    title: inputValue.value,
    done: false,
    subtasks: []
  })
  inputValue.value = ''
}
</script>

<style scoped lang="scss">
@import "@/assets/css/_includes.scss";

.task {
  padding: 20px;
  background: $background-secondary;
  border-radius: 10px;
  box-shadow: 0 2px 6px -4px rgba(0, 0, 0, 0.3);
  display: flex;
  align-items: flex-start;
  gap: 20px;
  cursor: pointer;
  transition: .2s background-color, .2s box-shadow;

  &__content {
    flex: 1;
    min-width: 0;
  }
  &__title {
    font-weight: 600;
    font-size: inherit;
  }
  &__desc {
    color: $elements-secondary;
    white-space: nowrap;
    max-width: 100%;
    overflow: hidden;
    text-overflow: ellipsis;
    margin-top: 10px;
  }
  &__bottom {
    margin-top: 15px;
  }

  &--done {
    opacity: .5;
    .task__title {
      text-decoration: line-through;
    }
  }

  &:hover {
    background: $background-tertiary;
  }

  &--create {
    cursor: default;
    &:hover {
      background: $background-secondary;
    }
  }
  &--create:not(&--creating) {
    background: transparent;
    box-shadow: none;
  }

  &--subtask {
    background: transparent;
    box-shadow: none;
    padding: 0 !important;
    gap: 15px;
    cursor: default;
    &:hover {
      background: transparent;
    }

    .task__title {
      font-weight: 400;
    }
  }

  @include mobile {
    padding: 10px;
    border-radius: 5px;
    gap: 10px;

    &__desc {
      margin-top: 5px;
    }
    &__bottom {
      margin-top: 10px;
    }
  }
}
</style>
