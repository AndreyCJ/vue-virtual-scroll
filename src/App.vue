<script setup lang="ts">
import VirtualScroll from './components/virtual-scroll.vue';
</script>

<template>
  <VirtualScroll />
</template>

<style>
body {
  margin: 0;
}
* {
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
