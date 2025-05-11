<script setup>
import { ref } from 'vue'
const props = defineProps({
    emoji: Object
})

const emojiButtonStatus = ref("Emoji")
const unicodeButtonStatus = ref("Unicode")
const highlighted = ref(false)

const actionButtonMessages = {
    "emoji": emojiButtonStatus,
    "unicode": unicodeButtonStatus
}

function handleEmoji() {
    copyToClipboard(`${props.emoji.emoji + '\u{fe0f}'}`, "emoji")
}

function handleUnicode() {
    copyToClipboard(props.emoji.unicode.toLowerCase(), "unicode")
}

async function copyToClipboard(value, caller) {
    let defaultMessage = actionButtonMessages[caller].value
    highlighted.value = true
    actionButtonMessages[caller].value = "Ok!"

    await navigator.clipboard.writeText(value)

    setTimeout(() => { 
        actionButtonMessages[caller].value = defaultMessage 
        highlighted.value = false
    }, 800)
}
</script>

<template>
    <div class="card-container" :class="{ highlight: highlighted }">
        <div class="emoji" @click="handleEmoji">{{ emoji.emoji + '\u{fe0f}' }}</div>
        <div class="shortname">{{ emoji.name.toLowerCase() }}</div>
        <div class="actions">
            <button class="button-copy copy-emoji hide-on-mobile" @click="handleEmoji">{{ emojiButtonStatus }}</button>
            <button class="button-copy copy-unicode hide-on-mobile" @click="handleUnicode">{{ unicodeButtonStatus }}</button>
        </div>
    </div>
</template>

<style scoped>
.card-container {
    display: flex;
    flex-direction: row;
    align-items: center;
    gap: 1.5em;
    padding: 1em 1em;
    border-bottom: 1px solid gray;
    overflow: hidden;
}

.emoji {
    width: 50px;
    font-family: 'Noto Sans', sans-serif;
    font-size: 2.5rem;
    text-align: center;
    user-select: none;
    cursor: pointer;
}

.shortname {
    flex: 1 0;
    font-size: 1.5rem;
    text-align: start;
}

.actions {
    margin: auto;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    gap: 1.5em;
}

.button-copy {
    max-width: 150px;
    font-size: 1.5rem;
    padding: 0.5rem 1rem;
}

@media (max-width: 600px) {
  .hide-on-mobile {
    display: none;
  }
}

.highlight {
    animation: on-selected 1s;
}

@keyframes on-selected {
    0% {
        background-color: #585858;
    }

    100% {
        background-color: inherit;
    }
}
</style>
