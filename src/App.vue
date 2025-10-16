<template>
  <div class="app">
    <GameCanvas />
  </div>
</template>

<script setup>
import GameCanvas from './components/GameCanvas.vue'
</script>

<style>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.cta {
  background: gold;
  border: none;
  font-weight: bold;
  cursor: pointer;
  border-radius: 6px;
}
</style>
