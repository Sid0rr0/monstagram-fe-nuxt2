<template>
  <vue-word-cloud
    :words="words"
    :color="([, weight]) => weight > 50 ? 'DeepPink' : weight > 10 ? 'purple' : 'RoyalBlue'"
    font-family="Roboto"
    :spacing="0.3"
  >
    <template slot-scope="{text, weight, word}">
      <div :title="weight" style="cursor: pointer;" @click="onWordClick(word)">
        {{ text }}
      </div>
    </template>
  </vue-word-cloud>
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

  methods: {
    onWordClick (word) {
      console.log(word)
    }
  }
}
</script>
