<template>
  <div>
    <input
      v-model="searchQuery"
      type="search"
      autocomplete="off"
      :placeholder="texts.placeholder"
      class="block w-full pl-10 pr-3 py-2 truncate leading-5 placeholder-gray-500 border border-gray-500 text-gray-700 focus:border-gray-300 rounded-md focus:outline-none focus:bg-white bg-white"
    />
    <ul
      v-if="articles.length"
      class="z-10 absolute w-auto flex-1 top-40 bg-white dark:bg-gray-900 rounded-md border border-gray-300 overflow-hidden"
    >
      <li v-for="article of articles" :key="article.slug">
        <NuxtLink
          :to="
            $store.state.data.lang === 'es'
              ? `/blog/${article.slug}`
              : `/en/blog/${article.slug}`
          "
          class="flex px-4 py-2 items-center leading-5 transition ease-in-out duration-150 text-green-500 hover:text-black"
        >
          {{ article.title }}
        </NuxtLink>
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  data() {
    return {
      searchQuery: '',
      articles: [],
      texts: {}
    }
  },
  watch: {
    async searchQuery(searchQuery) {
      if (!searchQuery) {
        this.articles = []
        return
      }
      this.articles = await this.$content(
        this.$store.state.data.lang,
        'articles'
      )
        .limit(6)
        .search('title', searchQuery)
        .fetch()
    }
  },
  async created() {
    const texts = await this.$content(
      this.$store.state.data.lang,
      'pages',
      'general'
    ).fetch()
    this.texts = texts.searchInput
  }
}
</script>
