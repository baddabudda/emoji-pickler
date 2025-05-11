<script setup>
import EmojiContainer from './components/EmojiCointainer.vue'
import json from './assets/emoji-data-no-mod.json'

import { ref, computed } from 'vue'

const search = ref("")
const items = computed(() => {
  return json.filter(item => item.name.toLowerCase().includes(search.value.toLowerCase()))
})
</script>

<template>
  <div class="header">
    <div class="name">
      Emoji pickler!
    </div>
    <div class="description">
      A place where you find emoji
    </div>
  </div>
 
  <div class="search-container">
    <input class="search" type="search" placeholder="Search by emoji name..." v-model="search">
  </div>

  <div class="emoji-list">
    <EmojiContainer v-for="item of items" :key="item.name" :emoji="item"/>
  </div>
</template>

<style scoped>
.header {
  text-align: center;
  word-wrap: break-word;
}

.name {
  font-size: 5.6rem;
}

.description {
  font-size: 2.4rem;
}

.search-container {
  padding: 3em 0;
  display: flex;
  justify-content: center;
}

.search {
  font-size: 2rem;
  padding: 0.25em 0.5em;
  width: 50vw;
  outline: none;
}
.emoji-list {
  max-width: 64rem;
  margin: auto;
  border-color: gray;
  border-style: solid;
  border-width: 1px;
  border-radius: 8px;
}
</style>
