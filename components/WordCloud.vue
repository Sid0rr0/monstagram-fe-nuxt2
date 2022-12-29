<template>
  <div class="mb-5">
    <vue-word-cloud
      :words="words"
      :color="([, weight]) => weight > 50 ? 'DeepPink' : weight > 10 ? 'purple' : 'RoyalBlue'"
      font-family="Helvetica"
      :spacing="0.3"
      style="height: 680px; width: 1440px;"
    >
      <template slot-scope="{text, weight, word}">
        <div :title="weight" style="cursor: pointer;" @click="onWordClick(word)">
          {{ text }}
        </div>
      </template>
    </vue-word-cloud>
    <div v-if="selectedWord.length > 0" class="flex flex-col">
      <h2 class="text-2xl mb-2">
        Příspěvky s hastagem <span class="text-blue-600">{{ selectedWord[0] }}</span>
      </h2>
      <a v-for="post in selectedWord[2]" :key="post" :href="`https://www.instagram.com/p/${post}/`" target="_blank">https://www.instagram.com/p/{{ post }}/</a>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VWordCloud',

  props: {
    words: {
      type: Array,
      required: true
    }
  },

  data () {
    return {
      selectedWord: []
    }
  },

  methods: {
    onWordClick (word) {
      console.log(word)
      this.selectedWord = word
    }
  }
}
</script>
