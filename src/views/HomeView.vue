<script setup lang="ts">
import MonacoEditor from '@/components/MonacoEditor.vue'
import HeaderApp from '@/components/HeaderApp.vue'
</script>

<template>
    <div class="bg-hero">
        <HeaderApp />
        <MonacoEditor />
    </div>
</template>

<style scoped>
.bg-hero {
    height: 100%;
    background: url('@/assets/Hero-Background-notecode@2x.png') no-repeat;
    background-position: center top;
    color: #121826;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 0px 8px;
}

@media (min-width: 1280px) {
    .bg-hero {
        background-size: contain;
    }
}
</style>