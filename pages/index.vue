<template>
  <main class="homepage m-auto">
    <h1 class="text-5xl mt-10 text-center">
      Analyzujte vaše sledující na Instagramu
    </h1>

    <div class="flex flex-col mt-8 w-6/12 m-auto">
      <div class="flex justify-between items-center">
        <span>Instagram handle:</span>
        <input v-model="handle" type="text" class="mt-3 px-1 border-2">
      </div>
      <div class="flex justify-between items-center">
        <span>Session ID:</span>
        <input type="text" class="mt-3 px-1 border-2">
      </div>
      <div class="flex justify-between items-center">
        <div />
        <button class="mt-3 px-3 py-1 border-2 rounded-full" @click="getWords">
          Enter
        </button>
      </div>
    </div>

    <word-cloud
      style="height: 680px; width: 1440px;"
      :words="words"
    />
  </main>
</template>

<script>
export default {
  name: 'IndexPage',

  data () {
    return {
      words: [],
      handle: ''
    }
  },

  methods: {
    async getWords () {
      if (!this.handle) {
        return
      }

      const words = await fetch(`http://127.0.0.1:8000/api/user/${this.handle}/word_counts`).then(res => res.json())
      const arr = Object.keys(words).map(key => [key, words[key]])
      const sorted = arr.sort((a, b) => b[1] - a[1]).slice(0, 100)
      const withLink = sorted.map(a => [...a, 'link'])

      this.words = withLink
    }
  }

}
</script>
