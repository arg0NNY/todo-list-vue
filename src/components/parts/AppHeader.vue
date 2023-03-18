<template>
  <header>
    <div class="container">
      <input type="text" class="header__title" v-model="title" placeholder="Add a title...">
      <div v-if="subtitle" class="header__subtitle">{{ subtitle }}</div>
    </div>
  </header>
</template>

<script setup>
import {useLocalStorage} from "@vueuse/core";

const props = defineProps({
  subtitle: String
})

const title = useLocalStorage('app-title', 'Welcome back!')
</script>

<style scoped lang="scss">
@import "@/assets/css/_includes.scss";

header {
  margin-top: 80px;
  margin-bottom: 40px;

  .header {
    &__title {
      font-size: 32px;
      font-weight: 600;
      background: none;
      border: none;
      display: block;
      width: 100%;
      border-bottom: 2px solid transparent;
      transition: .05s border-color;
      &:focus {
        outline: none;
        border-color: $elements-secondary;
      }
    }
    &__subtitle {
      margin-top: 15px;
      color: $elements-secondary;
    }
  }

  @include mobile {
    margin-top: 40px;
    margin-bottom: 20px;

    .header {
      &__title {
        font-size: 24px;
      }
      &__subtitle {
        margin-top: 5px;
      }
    }
  }
}
</style>
