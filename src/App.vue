<script setup lang="ts">
import ChatInput from './components/ChatInput.vue'
</script>

<template>
  <div class="app-container">
    <ChatInput />
  </div>
</template>

<style scoped>
.app-container {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2rem;
}
</style>
