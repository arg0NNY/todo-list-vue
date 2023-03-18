<template>
  <div class="modal-container" @mousedown.self="emit('close')">
    <div class="modal">
      <slot />
    </div>
  </div>
</template>

<script setup>
const emit = defineEmits(['close'])
</script>

<style scoped lang="scss">
@import "@/assets/css/_includes.scss";

.modal-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,.5);
}

.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: $background-secondary;
  border-radius: 15px;
  padding: 25px;
  width: 700px;
  box-shadow: 0 67px 27px rgba(0, 0, 0, 0.01), 0 37px 22px rgba(0, 0, 0, 0.05), 0 17px 17px rgba(0, 0, 0, 0.09), 0 4px 9px rgba(0, 0, 0, 0.1), 0 0 0 rgba(0, 0, 0, 0.1);
  max-height: 90vh;
  overflow-x: hidden;
  overflow-y: auto;
}
</style>
