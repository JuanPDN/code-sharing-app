<script setup lang="ts">
import { ref, onMounted } from 'vue';
import * as monaco from 'monaco-editor/esm/vs/editor/editor.api';

const monacoEditor = ref<HTMLElement | null>(null);

onMounted(() => {
    if (monacoEditor.value) {
        monaco.editor.create(monacoEditor.value, {
            value: 'Hello, World!',
            language: 'HTML',
        })
    }
})

</script>

<template>
    <div class="container">
        <div ref="monacoEditor" class="monaco"></div>
    </div>
</template>

<style scoped>
.container {
    width: 100%;
    max-width: 880px;
    margin: 0px 8px 80px 8px;
    padding: 24px 16px 16px 16px;
    background: rgb(255, 255, 254);
    border-radius: 8px;
    box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
}


.monaco {
    width: 100%;
    height: 720px;
}
</style>