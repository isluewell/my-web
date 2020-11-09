<template>
  <post :article="article" :tags="tags" :prev="prev" :next="next"></post>
</template>
<script>
export default {
  async asyncData({ $content, params, app }) {
    // const lang = 'en'
    // const article = await $content(lang, 'articles', params.slug).fetch()
    // const tagsList = await $content('tags')
    //   .only(['name', 'slug'])
    //   .where({ name: { $containsAny: article.tags } })
    //   .fetch()
    // const tags = Object.assign({}, ...tagsList.map((s) => ({ [s.name]: s })))
    // const [prev, next] = await $content(lang, 'articles')
    //   .only(['title', 'slug'])
    //   .sortBy('createdAt', 'asc')
    //   .surround(params.slug)
    //   .fetch()
    // return {
    //   article,
    //   tags,
    //   prev,
    //   next
    // }
  },
  data() {
    return {
      article: {},
      tags: {},
      prev: {},
      next: {}
    }
  },
  async created() {
    const article = await this.$content(
      this.$store.state.data.lang,
      'articles',
      this.$route.params.slug
    ).fetch()
    const tagsList = await this.$content('tags')
      .only(['name', 'slug'])
      .where({ name: { $containsAny: article.tags } })
      .fetch()
    const tags = Object.assign({}, ...tagsList.map((s) => ({ [s.name]: s })))
    const [prev, next] = await this.$content(
      this.$store.state.data.lang,
      'articles'
    )
      .only(['title', 'slug'])
      .sortBy('createdAt', 'asc')
      .surround(this.$route.params.slug)
      .fetch()

    this.article = article
    this.tags = tags
    this.prev = prev
    this.next = next
  },
  head() {
    return {
      title: 'Blog - ' + this.article.title,
      meta: [
        {
          hid: 'viewport',
          name: 'viewport',
          content: 'width=device-width, initial-scale=1'
        },
        {
          hid: 'theme-color',
          name: 'theme-color',
          content: 'black'
        },
        {
          hid: 'theme-color',
          name: 'theme-color',
          content: 'black'
        },
        {
          hid: 'apple-mobile-web-app-title',
          name: 'apple-mobile-web-app-title',
          content: 'Blog - ' + this.article.title
        },
        {
          hid: 'description',
          name: 'description',
          content: this.article.description
        },
        {
          hid: 'canonical',
          rel: 'canonical',
          href: process.env.BASE_URL + this.$route.path
        },
        { src: '/img/guillermo-card.png', alt: 'xyz' },
        { property: 'og:type', content: 'blog' },
        {
          hid: 'og:title',
          name: 'og:title',
          property: 'og:title',
          content: 'Blog - ' + this.article.title
        },
        { property: 'og:description', content: this.article.description },
        { property: 'og:image', content: this.article.img },
        {
          property: 'og:url',
          content: process.env.BASE_URL + this.$route.path
        },
        { property: 'og:site_name', content: 'Blog - ' + this.article.title },
        { name: 'twitter:title', content: 'Blog - ' + this.article.title },
        { name: 'twitter:description', content: this.article.description },
        { name: 'twitter:image', content: this.article.img },
        {
          name: 'twitter:site',
          content: process.env.BASE_URL + this.$route.path
        },
        { name: 'twitter:creator', content: '@GuillermoDani25' }
      ]
    }
  }
}
</script>
