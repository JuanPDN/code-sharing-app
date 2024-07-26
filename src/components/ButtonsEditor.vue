<script setup lang="ts">
import { useRoute, useRouter } from 'vue-router';
import { ref } from 'vue';

const textToCopy = ref<HTMLElement | null>(null)
const route = useRoute()
const router = useRouter()
const emit = defineEmits(['update:theme-selected', 'update:language-selected']);
const props = defineProps({
    value: String,
    theme: String,
    laguage: String
})

const initialValue = ref("")

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
    navigator.clipboard.writeText(fullPath)
}

const share = async () => {
    const { value, theme, laguage } = props
    const id = route.params.id
    initialValue.value = value!
    if (id) {
        await fetch(`http://localhost:3000/api/code/${id}`, {
            method: 'PUT',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
                code: value,
                theme: theme,
                language: laguage
            })
        })
    } else {
        await fetch(`http://localhost:3000/api/code`, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify({
                code: value,
                theme: theme,
                language: laguage
            })
        }).then((data) => {
            data.json().then((data) => {
                router.push({ name: 'code', params: { id: data } })
            })
        })
    }
    copy()
}

</script>

<template>
    <section>
        <div class="btns-select">
            <select :value="laguage" name="language" id="language" @change="setLanguage($event)">
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
            <button class="btn-share" @click="share" :disabled="initialValue === value">
                <img src="@/assets/Share.svg" alt="share" height="16" width="16">
                <span>Share</span>
            </button>
        </div>
    </section>
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