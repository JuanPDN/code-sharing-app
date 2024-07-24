<script setup lang="ts">
import '@/utils/workers'

import { ref, onMounted, onBeforeUnmount, watch } from 'vue';
import * as monaco from 'monaco-editor/esm/vs/editor/editor.api';

import { defaultCode } from '@/utils/defaultCode';
import ButtonsEditor from '@/components/ButtonsEditor.vue';

const monacoEditor = ref<HTMLElement | null>(null);
let editor: monaco.editor.IStandaloneCodeEditor | null = null;
const theme = ref('vs-light');
const language = ref('html');
const code = ref(defaultCode);


onMounted(() => {
    if (monacoEditor.value) {
        editor = monaco.editor.create(monacoEditor.value, {
            value: code.value,
            language: language.value,
            automaticLayout: true,
            scrollbar: {
                vertical: 'auto',
            },
            hideCursorInOverviewRuler: true,
            overviewRulerBorder: false,
            theme: theme.value
        })

        editor.onDidChangeModelContent(() => {
            code.value = editor!.getValue();
        })
    }
})

onBeforeUnmount(() => {
    if (editor) {
        editor.dispose();
    }
})

const updateTheme = (event: string) => {
    theme.value = event
}

const updateLanguage = (event: string) => {
    language.value = event
}


watch(theme, (theme) => editor!.updateOptions({ theme: theme }))
watch(language, () => {
    if (editor) {
        monaco.editor.setModelLanguage(editor.getModel()!, language.value)
    }
})

</script>

<template>
    <div class="container" :class="theme">
        <div ref="monacoEditor" data-lang="html" class="monaco"></div>
        <ButtonsEditor @update:theme-selected="updateTheme($event)" :theme="theme" :laguage="language" :value="code"
            @update:language-selected="updateLanguage($event)" />
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
    border-radius: 8px;
    box-shadow: 0px 10px 15px -3px rgba(0, 0, 0, 0.1);
}

.vs-light {
    background: rgb(255, 255, 254);
}

.vs-dark {
    background: rgb(30, 30, 30);
}

.hc-black {
    background: rgb(0, 0, 0);
}

.monaco {
    height: 100%;
    width: 100%;
    padding-right: 16px;

}
</style>