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
        <button class="mt-3 px-3 py-1 border-2 rounded-full" @click="getTags">
          Tagy
        </button>
        <button class="mt-3 px-3 py-1 border-2 rounded-full" @click="getWords">
          Slova
        </button>
      </div>
    </div>

    <div v-if="(allWords.length > 0)">
      Celkový počet slov: {{ allWords.length }}, počet slov s četností alspoň 5: {{ limitedWords.length }}
    </div>

    <word-cloud
      v-if="(words.length > 0)"
      style="height: 680px; width: 1440px;"
      :words="words"
    />

    <word-cloud
      v-if="tags.length > 0"
      style="height: 680px; width: 1440px;"
      :words="tags"
    />

    <div class="flex flex-row">
      <div>
        <table v-if="(allWords.length > 0)">
          <tr>
            <td>Četnost</td>
            <td class="pl-3">
              Slovo
            </td>
          </tr>
          <tr v-for="(word, i) in limitedWords" :key="i">
            <td>{{ word[1] }}</td>
            <td class="pl-3">
              {{ word[0] }}
            </td>
          </tr>
        </table>
      </div>
      <div>
        <table v-if="(tags.length > 0)">
          <tr>
            <td>Četnost</td>
            <td class="pl-3">
              Tag
            </td>
          </tr>
          <tr v-for="(tag, i) in tags" :key="i">
            <td>{{ tag[1] }}</td>
            <td class="pl-3">
              {{ tag[0] }}
            </td>
          </tr>
        </table>
      </div>
    </div>
  </main>
</template>

<script>
export default {
  name: 'IndexPage',

  data () {
    return {
      words: [],
      allWords: [],
      limitedWords: [],
      handle: '_just_meri_',
      tags: []
    }
  },

  methods: {
    async getWords () {
      if (!this.handle) {
        return
      }

      const words = await fetch(`${this.$config.apiURL}/user/${this.handle}/word_counts`).then(res => res.json())
      const arr = Object.keys(words).map(key => [key, words[key]])
      const sorted = arr.sort((a, b) => b[1] - a[1])
      const withLink = sorted.slice(0, 100).map(a => [...a, 'link'])

      this.allWords = sorted
      this.limitedWords = sorted.filter(word => word[1] >= 5)
      this.words = withLink
    },

    async getTags () {
      if (!this.handle) {
        return
      }

      const words = await fetch(`${this.$config.apiURL}/user/${this.handle}/tags`).then(res => res.json())
      const arr = []
      for (const [key, value] of Object.entries(words)) {
        arr.push([key, ...Object.values(value)])
      }

      this.tags = arr.sort((a, b) => b[1] - a[1])
    }
  }

}
</script>
