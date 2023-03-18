<template>
  <div class="chip">
    <slot />
  </div>
</template>

<script setup>

</script>

<style lang="scss">
@import "@/assets/css/_includes.scss";

.chip {
  border: 1px solid $elements-quaternary;
  border-radius: 7px;
  padding: 3px 5px;
  display: inline-flex;
  align-items: center;
  gap: 5px;
  color: $elements-secondary;
  font-size: 14px;
  user-select: none;

  .icon {
    height: 16px;
    width: 16px;
  }
}
</style>
