<script setup lang="ts">
import '@/utils/workers'

import { ref, onMounted, onBeforeUnmount, watch } from 'vue';
import * as monaco from 'monaco-editor/esm/vs/editor/editor.api';

import { defaultCode } from '@/utils/defaultCode';
import ButtonsEditor from '@/components/ButtonsEditor.vue';
import { useRoute, useRouter } from 'vue-router';

const monacoEditor = ref<HTMLElement | null>(null);
let editor: monaco.editor.IStandaloneCodeEditor | null = null;
const theme = ref("vs-light");
const language = ref("html");
const code = ref(defaultCode);
const api_url = import.meta.env.VITE_API_URL
const route = useRoute();
const router = useRouter();
const id = route.params.id;

const fetchCode = async (codeId: string) => {
    const response = await fetch(`${api_url}${codeId}`);
    const data = await response.json();
    return data
}

onMounted(async () => {
    if (id) {
        try {
            const data = await fetchCode(id.toString())
            code.value = data.code + " "
            theme.value = data.theme
            language.value = data.language
        } catch (error) {
            router.push({ name: 'home' });
        }
    }
    if (monacoEditor.value) {
        editor = monaco.editor.create(monacoEditor.value, {
            value: code.value,
            language: language.value,
            theme: theme.value,
            automaticLayout: true,
            minimap: {
                autohide: visualViewport!.width < 640,
            },
            scrollbar: {
                vertical: 'auto',
            },
            hideCursorInOverviewRuler: true,
            overviewRulerBorder: false,
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

const updateTheme = (event: string) => theme.value = event;
const updateLanguage = (event: string) => language.value = event;

watch(theme, (theme) => editor!.updateOptions({ theme: theme }))
watch(language, () => {
    if (editor) {
        monaco.editor.setModelLanguage(editor.getModel()!, language.value)
    }
})

</script>

<template>
    <div class="container" :class="theme">
        <div ref="monacoEditor" class="monaco"></div>
        <ButtonsEditor @update:theme-selected="updateTheme" @update:language-selected="updateLanguage" :theme="theme"
            :language="language" :code="code" />
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