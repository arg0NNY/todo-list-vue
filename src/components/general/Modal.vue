<template>
  <div class="modal-container" @mousedown.self="emit('close')">
    <div class="modal">
      <button class="modal-close-btn" @click="emit('close')"><IconX/></button>
      <div class="modal__content">
        <template v-if="common">
          <div class="modal__title"><slot name="title" /></div>
          <div class="modal__desc"><slot /></div>
          <div class="modal__actions"><slot name="actions" /></div>
        </template>
        <slot v-else />
      </div>
    </div>
  </div>
</template>

<script setup>
import {useScrollLock} from "@vueuse/core"
import {onBeforeUnmount, onMounted} from "vue"
import IconX from "@/components/icons/IconX.vue"

const props = defineProps({
  common: Boolean,
  submodal: Boolean
})

const emit = defineEmits(['close'])

if (!props.submodal) {
  const isLocked = useScrollLock(document.body)

  onMounted(() => isLocked.value = true)
  onBeforeUnmount(() => isLocked.value = false)
}
</script>

<style lang="scss">
@import "@/assets/css/_includes.scss";

.modal-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0,0,0,.5);
  z-index: 100000;
}

.modal-close-btn {
  flex-shrink: 0;
  width: 30px;
  height: 30px;
  background: none;
  border: none;
  cursor: pointer;
  display: flex;
  justify-content: center;
  align-items: center;
  color: $background-secondary;
  transition: .2s opacity;

  .icon {
    width: 20px;
    height: 20px;
  }

  &:hover {
    opacity: .6;
  }
  &:active {
    opacity: .4;
  }
}

.modal {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  max-height: 90vh;
  width: 700px;
  display: flex;
  flex-direction: column;
  align-items: flex-end;

  &__content {
    width: 100%;
    padding: 25px;
    overflow-x: hidden;
    overflow-y: auto;
    background: $background-secondary;
    border-radius: 15px;
    box-shadow: 0 67px 27px rgba(0, 0, 0, 0.01), 0 37px 22px rgba(0, 0, 0, 0.05), 0 17px 17px rgba(0, 0, 0, 0.09), 0 4px 9px rgba(0, 0, 0, 0.1), 0 0 0 rgba(0, 0, 0, 0.1);
  }

  &__title {
    font-size: 24px !important;
    font-weight: 700 !important;
  }
  &__desc {
    margin-top: 10px;
  }
  &__actions {
    margin-top: 20px;
    display: flex;
    flex-direction: row-reverse;
    align-items: center;
    gap: 10px;
  }

  @include mobile {
    width: calc(100% - 30px);

    &__content {
      border-radius: 10px;
      padding: 15px;
    }
    &__title {
      font-size: 18px !important;
    }
    &__desc {
      margin-top: 7px;
    }
    &__actions {
      margin-top: 15px;
      gap: 5px;
    }
  }
}
</style>
