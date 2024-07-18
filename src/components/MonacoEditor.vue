<script setup lang="ts">
import { ref, onMounted } from 'vue';
import * as monaco from 'monaco-editor/esm/vs/editor/editor.api';
import { defaultCode } from '@/utils/defaultCode';
import ButtonsEditor from '@/components/ButtonsEditor.vue';

const monacoEditor = ref<HTMLElement | null>(null);


onMounted(() => {
    if (monacoEditor.value) {
        monaco.editor.create(monacoEditor.value, {
            value: defaultCode,
            language: 'html',
            automaticLayout: true
        })
    }
})

</script>

<template>
    <div class="container">
        <div ref="monacoEditor" class="monaco"></div>
        <ButtonsEditor />
    </div>
</template>

<style scoped>
.container {
    display: flex;
    flex-direction: column;
    align-items: center;
    max-width: 880px;
    width: 100%;
    height: 720px;
    margin: 0px 8px 80px 8px;
    padding-top: 24px;
    background: rgb(255, 255, 254);
    border-radius: 8px;
    box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
}

.monaco {
    height: 100%;
    width: 100%;
    padding-right: 16px;

}
</style>