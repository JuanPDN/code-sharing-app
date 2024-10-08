<script setup lang="ts">
import { useRoute, useRouter } from 'vue-router';
import { computed, ref, watch } from 'vue';
import ShareWindow from '@/components/ShareWindow.vue';

const textToCopy = ref<HTMLElement | null>(null)
const route = useRoute()
const router = useRouter()
const id = ref(route.params.id)
const emit = defineEmits(['update:theme-selected', 'update:language-selected']);
const props = defineProps({
    code: String,
    theme: String,
    language: String
})

const initialCode = ref(id.value ? props.code : '')
const initialTheme = ref(id.value ? props.theme : '')
const initialLanguage = ref(id.value ? props.language : '')
const isShareVisible = ref(false)

const isDisabled = computed(() => (
    initialCode.value === props.code &&
    initialTheme.value === props.theme &&
    initialLanguage.value === props.language &&
    route.params.id !== undefined
))

const setTheme = (theme: Event) => {
    const themeSelect = theme.target as HTMLSelectElement
    emit('update:theme-selected', themeSelect.value)
}

const setLanguage = (language: Event) => {
    const languageSelect = language.target as HTMLSelectElement
    emit('update:language-selected', languageSelect.value)
}

const copy = () => {
    const fullPath = window.location.origin + route.fullPath
    if (textToCopy.value) {
        navigator.clipboard.writeText(fullPath).then(() => {
            textToCopy.value!.textContent = 'Copied to clipboard!'
        }).catch((error) => {
            console.error(error)
            alert('Failed to copy!')
        })
        setTimeout(() => {
            textToCopy.value!.textContent = `...${route.fullPath}`
        }, 1500);
    }
}

const updateVisible = (value: boolean) => {
    isShareVisible.value = value
}

const share = async () => {
    const { code, theme, language } = props
    initialCode.value = code
    initialTheme.value = theme
    initialLanguage.value = language

    const body = JSON.stringify({ code, theme, language })
    const optionsFetch = { method: id.value ? 'PUT' : 'POST', headers: { 'Content-Type': 'application/json' }, body }
    const api_url = import.meta.env.VITE_API_URL

    if (id.value) {
        await fetch(`${api_url}${id.value}`, optionsFetch)
    } else {
        await fetch(api_url, optionsFetch).then((data) => {
            data.json().then((data) => {
                id.value = data
                router.push({ name: 'code', params: { id: data } })
            })
        })
    }
    isShareVisible.value = true
}
watch(props, (newProps) => {
    initialCode.value = newProps.code;
    initialTheme.value = newProps.theme;
    initialLanguage.value = newProps.language;
}, { once: true });

</script>

<template>
    <section>
        <div class="btns-select">
            <select :value="language" name="language" id="language" @change="setLanguage($event)">
                <option value="html">HTML</option>
                <option value="css">CSS</option>
                <option value="javascript">Javascript</option>
                <option value="typescript">Typescript</option>
                <option value="json">Json</option>
            </select>
            <select :value="theme" name="theme" id="theme" @change="setTheme($event)">
                <option value="vs-light">Light</option>
                <option value="vs-dark">VS Dark</option>
                <option value="hc-black">HC Dark</option>
            </select>
        </div>
        <div class="container-btns">
            <div v-if="route.params.id">
                <button class="btn-copy" @click="copy">
                    <img src="@/assets/link.svg" alt="link" height="24" width="24">
                    <span :class="theme !== 'vs-light' && 'dark'" ref="textToCopy">...{{
                        $route.fullPath }}</span>
                </button>
            </div>
            <button class="btn-share" @click="share" :disabled="isDisabled">
                <img src="@/assets/Share.svg" alt="share" height="16" width="16">
                <span>Share</span>
            </button>
        </div>
    </section>
    <ShareWindow v-if="id" @update:isShareVisible="updateVisible" :fullPath=id.toString()
        :isShareVisible="isShareVisible" />
</template>

<style scoped>
.container-btns {
    display: flex;
    align-items: end;
    gap: 16px;
}

.btn-copy {
    padding: 5px 0px;
    display: flex;
    align-items: center;
    gap: 12px;
    appearance: none;
    border: none;
    background-color: transparent;
    font-size: 10px;
    font-weight: 600;
    color: rgba(54, 65, 82, 0.7);
    cursor: copy;
}

.btn-copy span {
    display: none;
}

.dark {
    color: rgb(102, 116, 137);
}

@media (min-width: 550px) {
    .btn-copy span {
        display: block;
    }
}


select {
    appearance: none;
    font-weight: 600;
    font-size: 10px;
    padding: 6px 24px 6px 12px;
    background: #CED6E1 url('@/assets/down-arrow.svg') no-repeat calc(100% - 8px) center;
    color: #364153;
    border: none;
    outline: none;
    border-radius: 15px;
}

section {
    width: 100%;
    display: flex;
    justify-content: space-between;
    align-items: end;
    padding: 0px 16px 16px 16px;
}

.btn-share {
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 16px;
    font-weight: 600;
    color: #FFFFFE;
    background-color: #406AFF;
    gap: 8px;
    padding: 12px 24px;
    border-radius: 25px;
    border: none;
    cursor: pointer;
}

.btn-share:disabled {
    background-color: rgb(104, 115, 138);
    cursor: not-allowed;
}

.btns-select {
    display: flex;
    gap: 12px;
}
</style>