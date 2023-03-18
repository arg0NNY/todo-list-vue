<template>
  <button class="btn" :class="classes">
    <slot />
  </button>
</template>

<script setup>
import {computed} from "vue"

const props = defineProps({
  danger: Boolean,
  primary: Boolean
})

const classes = computed(() => ['danger', 'primary'].filter(i => !!props[i]).map(i => `btn-${i}`))
</script>

<style lang="scss">
@import "@/assets/css/_includes.scss";

.btn {
  background: $background-secondary;
  color: $elements-secondary;
  border: 1px solid $elements-quaternary;
  border-radius: 10px;
  padding: 0 15px;
  height: 40px;
  font-size: 16px;
  font-weight: 500;
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 7px;
  cursor: pointer;
  box-shadow: 0 3px 12px -7px rgba(0, 0, 0, 0.2);
  transition: .2s background, .1s filter, .2s color;

  .icon {
    width: 20px;
    height: 20px;
  }

  &:hover {
    background: $background-tertiary;
  }
  &-danger:hover {
    background: $color-danger;
    color: $background-secondary;
  }
  &-primary:hover {
    background: $accent-primary;
    color: $background-secondary;
  }

  &:active {
    filter: brightness(.95);
  }
}
</style>
