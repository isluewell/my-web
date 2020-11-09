<template>
  <header class="flex items-center justify-between py-2 px-2 border-b">
    <nuxt-link to="/"
      ><Logo
        :config="{
          class: 'NuxtLogo',
          width: '40',
          height: '40'
        }"
    /></nuxt-link>
    <ul class="inline-flex items-center">
      <li class="px-2 md:px-4">
        <app-search-input></app-search-input>
      </li>
      <li class="px-2 md:px-4">
        <nuxt-link
          :to="$store.state.data.lang === 'es' ? '/en/blog' : '/blog'"
          class="text-purple-600 font-semibold hover:text-purple-500"
        >
          {{ $store.state.data.lang === 'es' ? 'EN' : 'ES' }}
        </nuxt-link>
      </li>
      <li v-if="$route.params.slug" class="px-2 md:px-4">
        <nuxt-link
          :to="$store.state.data.lang === 'es' ? '/blog' : '/en/blog'"
          class="text-black-600 font-semibold hover:text-gray-700"
        >
          {{ texts.text1 }}
        </nuxt-link>
      </li>
    </ul>
  </header>
</template>
<script>
export default {
  data() {
    return {
      texts: {}
    }
  },
  async created() {
    const texts = await this.$content(
      this.$store.state.data.lang,
      'pages',
      'general'
    ).fetch()
    this.texts = texts.navBlog
  }
}
</script>
