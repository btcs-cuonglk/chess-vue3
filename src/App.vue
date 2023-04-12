<script setup lang="ts">
// import { RouterLink, RouterView } from 'vue-router'
import TodoList from "./views/TodoList/index.vue";
</script>

<template>
  <header>
    <h1 class="text-center font-bold text-3xl my-5 text-white">Chess</h1>
  </header>

  <TodoList></TodoList>
  <!-- <RouterView /> -->
</template>

<style scoped>
header {
}
</style>
