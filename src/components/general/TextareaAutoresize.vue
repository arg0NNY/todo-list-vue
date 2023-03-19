<template>
  <textarea ref="textarea" @input="resizeTextarea" v-model="model" @keydown.enter="onEnter"></textarea>
</template>

<script setup>
import {computed, onMounted, ref, watch} from "vue"
import {useResizeObserver} from "@vueuse/core";

const props = defineProps({
  modelValue: String,
  allowLinebreaks: Boolean
})
const emit = defineEmits(['update:modelValue'])

const model = computed({
  get: () => props.modelValue,
  set: value => {
    if (!props.allowLinebreaks)
      value = value
        .replaceAll('\n', ' ')
        .replaceAll(/\s{2,}/ug, ' ')

    emit('update:modelValue', value)
  }
})

function onEnter(e) {
  if (props.allowLinebreaks) return

  e.preventDefault()
  e.target.form?.dispatchEvent(
      new Event("submit", { cancelable: true })
  )
}

const textarea = ref()
function resizeTextarea() {
  if (!textarea.value) return
  textarea.value.style.height = "auto"
  textarea.value.style.height = textarea.value.scrollHeight + "px"
}

useResizeObserver(document.body, () => resizeTextarea())
watch(model, () => setTimeout(resizeTextarea))
onMounted(() => resizeTextarea())
</script>

<style scoped lang="scss">
textarea {
  resize: none;
  overflow: hidden;
}
</style>
