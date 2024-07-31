<script setup lang="ts">
import { ref, watch } from 'vue';

const emit = defineEmits(['update:isShareVisible'])
const props = defineProps({
    fullPath: String,
    isShareVisible: Boolean
})

const url = ref(`${window.location.origin}/${props.fullPath}`)
const shareWidnow = ref<HTMLElement | null>(null)

const copyUrl = () => {
    navigator.clipboard.writeText(url.value).then(() => {
        url.value = 'Copied to clipboard!'
        setTimeout(() => {
            url.value = `${window.location.origin}/${props.fullPath}`
        }, 1500);
    }).catch((error) => {
        console.error(error)
        alert('Failed to copy!')
    })
}

const closeWindow = () => emit('update:isShareVisible', false)


watch(props, (newProps) => {
    url.value = `${window.location.origin}/${newProps.fullPath}`
});

</script>
<template>
    <div v-if="isShareVisible" ref="shareWidnow" class="container">
        <button class="btn-close" @click="closeWindow">X</button>
        <input type="text" name="url" id="url" :value=url readonly>
        <button class="btn-copy" @click="copyUrl">
            <img src="@/assets/link.svg" alt="copy" height="24" width="24">
        </button>
    </div>
</template>
<style scoped>
.container {
    background-image: linear-gradient(90deg, rgba(157, 108, 239), rgba(124, 70, 230));
    position: fixed;
    z-index: 5;
    box-shadow: 0 0 0 9999px rgba(0, 0, 0, 0.7);
    display: flex;
    gap: 16px;
    padding: 16px;
    border-radius: 8px;
    width: 90%;
    max-width: 500px;
}

.btn-copy {
    background-color: rgba(255, 255, 255, 0.2);
    border-radius: 5px;
    padding: 3px 5px;
    transition: all 0.2s ease-in-out;
}

.btn-close {
    position: absolute;
    padding: 5px;
    height: 30px;
    width: 30px;
    border-radius: 100%;
    top: -20px;
    right: -20px;
    font-size: 20px;
    background-color: rgba(255, 255, 255, 0.8);
    transition: all 0.2s ease-in-out;
}

button:hover {
    scale: 1.1;
}

input {
    border-radius: 5px;
    appearance: none;
    border: rgb(133, 63, 255) 3px solid;
    outline: none;
    color: #FFFFFE;
    font-size: 16px;
    font-weight: 400;
    width: 100%;
    background-color: rgba(255, 255, 255, 0.2);
    padding: 8px;
}

img {
    filter: brightness(3);
}
</style>