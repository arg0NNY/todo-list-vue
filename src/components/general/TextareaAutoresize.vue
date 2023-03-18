<template>
  <textarea ref="textarea" @input="resizeTextarea" v-model="model"></textarea>
</template>

<script setup>
import {computed, onMounted, ref, watch} from "vue"
import {useResizeObserver} from "@vueuse/core";

const props = defineProps(['modelValue'])
const emit = defineEmits(['update:modelValue'])

const model = computed({
  get: () => props.modelValue,
  set: value => emit('update:modelValue', value)
})

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
