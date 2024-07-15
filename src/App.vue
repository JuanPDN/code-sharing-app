<script setup lang="ts">

</script>

<template>
  <div class="bg-hero">
    "hello world"
  </div>

</template>

<style scoped>
.bg-hero {
  height: 100vh;
  background: url('./assets/Hero-Background-notecode.svg') no-repeat;
  background-position: center top;
}

@media (min-width: 1280px) {
  .bg-hero {
    background-size: cover;
  }
}
</style>
