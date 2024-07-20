<script setup lang="ts">

const emit = defineEmits(['update:theme-selected', 'update:language-selected'])

const setTheme = (theme: Event) => {
    const themeSelect = theme.target as HTMLSelectElement
    emit('update:theme-selected', themeSelect.value)
}

const setLanguage = (language: Event) => {
    const languageSelect = language.target as HTMLSelectElement
    emit('update:language-selected', languageSelect.value)
}

</script>

<template>
    <section>
        <div class="btns-select">
            <select name="language" id="language" @change="setLanguage($event)">
                <option value="html">HTML</option>
                <option value="css">CSS</option>
                <option value="javascript">Javascript</option>
                <option value="typescript">Typescript</option>
                <option value="json">Json</option>
            </select>
            <select name="theme" id="theme" @change="setTheme($event)">
                <option value="vs-light">Light</option>
                <option value="vs-dark">VS Dark</option>
                <option value="hc-black">HC Dark</option>
            </select>
        </div>
        <div>
            <button class="btn-share">
                <img src="@/assets/Share.svg" alt="share" height="16" width="16">
                <span>Share</span>
            </button>
        </div>
    </section>
</template>

<style scoped>
select {
    appearance: none;
    font-weight: 600;
    font-size: 10px;
    padding: 4px 24px 4px 12px;
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

.btns-select {
    display: flex;
    gap: 12px;
}
</style>