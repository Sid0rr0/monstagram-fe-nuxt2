<template>
  <div class="app flex flex-col min-h-screen">
    <header class="bg-purple-400 p-2 flex flex-row items-center justify-between">
      <NuxtLink to="/" class="flex flex-row items-center">
        <img alt="Logo" :src="require('~/assets/img/monstagram.svg')" class="w-8 h-8 mr-2"> Monstagram
      </NuxtLink>
      <div class="">
        <!-- <NuxtLink to="/" class="">

        </NuxtLink> -->
        <NuxtLink to="about" class="">
          O Monstagramu
        </NuxtLink>
      </div>
    </header>
    <Nuxt class="page" />
    <div class="flex-1" />
    <Footer />
  </div>
</template>

<script>
export default {
  head: {
    title: 'Monstagram'
  }
}
</script>
