<template>
  <button class="checkbox" :class="{ 'checkbox--checked': model }" @click.stop="model = !model">
    <IconCheck class="checkbox__check" />
  </button>
</template>

<script setup>
import IconCheck from "@/components/icons/IconCheck.vue"
import {computed} from "vue";

const props = defineProps({
  modelValue: Boolean
})
const emit = defineEmits(['update:modelValue'])

const model = computed({
  get: () => props.modelValue ?? false,
  set: value => emit('update:modelValue', value)
})
</script>

<style scoped lang="scss">
@import "@/assets/css/_includes.scss";

.checkbox {
  background: $background-secondary;
  border: 1px solid $elements-quaternary;
  border-radius: 3px;
  box-shadow: 0 2px 6px -4px rgba(0, 0, 0, 0.2);
  display: flex;
  justify-content: center;
  align-items: center;
  width: 20px;
  height: 20px;
  cursor: pointer;
  color: $background-secondary;

  &__check {
    width: 16px;
    opacity: 0;
  }

  &--checked {
    background: $accent-primary;
    border-color: $accent-primary;
    .checkbox__check {
      opacity: 1;
    }
  }
}
</style>
