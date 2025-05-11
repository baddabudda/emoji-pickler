<script setup>
import { ref } from 'vue'
const props = defineProps({
    emoji: Object
})

const emojiButtonStatus = ref("Emoji")
const unicodeButtonStatus = ref("Unicode")

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
    actionButtonMessages[caller].value = "Ok!"

    await navigator.clipboard.writeText(value)

    setTimeout(() => { actionButtonMessages[caller].value = defaultMessage}, 1000)
}
</script>

<template>
    <div class="card-container">
        <div class="emoji">{{ emoji.emoji + '\u{fe0f}' }}</div>
        <div class="shortname">{{ emoji.name.toLowerCase() }}</div>
        <div class="actions">
            <button class="button-copy copy-emoji" @click="handleEmoji">{{ emojiButtonStatus }}</button>
            <button class="button-copy copy-unicode" @click="handleUnicode">{{ unicodeButtonStatus }}</button>
        </div>
    </div>
</template>

<style scoped>
.card-container {
    display: flex;
    flex-direction: row;
    align-items: center;
    flex-wrap: wrap;
    gap: 1.5em;
    padding: 1em 1em;
    border-bottom: 1px solid gray;
    overflow: hidden;
}

.emoji {
    flex: 1 0 5%;
    width: 50px;
    font-family: 'Noto Sans', sans-serif;
    font-size: 2.5rem;
    text-align: center;
}

.shortname {
    flex: 1 0 50%;
    font-size: 1.5rem;
    text-align: start;
}

.actions {
    flex: 1 1 40%;
    margin: auto;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    gap: 1.5em;
}

.button-copy {
    width: 150px;
    font-size: 1.5rem;
    padding: 0.5rem 1rem;
}

.copy-emoji {
    margin-left: auto;
}
</style>
