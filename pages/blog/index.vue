<template>
  <posts :articles="articles"></posts>
</template>
<script>
export default {
  async asyncData({ $content, params, store }) {
    const lang = 'es'
    const articles = await $content(lang, 'articles', params.slug)
      .only(['title', 'description', 'img', 'slug', 'author', 'tags'])
      .sortBy('createdAt', 'desc')
      .fetch()
    // const tags = await $content('tags', params.slug)
    //   .only(['name', 'description', 'img', 'slug'])
    //   .sortBy('createdAt', 'asc')
    //   .fetch()
    return {
      articles
      // tags
    }
  },
  data() {
    return {
      title: 'Blog - Guillermo Daniel',
      description:
        'Opiniones, Reseñas, Programación cualquier cosa que se pueda escribir, espero lo encuentres aqui.'
    }
  },
  created() {
    this.$store.commit('data/SET_LANG', 'es')
  },
  head() {
    return {
      title: this.title,
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
          content: this.title
        },
        {
          hid: 'description',
          name: 'description',
          content: this.description
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
          content: this.title
        },
        { property: 'og:description', content: this.description },
        { property: 'og:image', content: '/img/guillermo-card.png' },
        {
          property: 'og:url',
          content: process.env.BASE_URL + this.$route.path
        },
        { property: 'og:site_name', content: this.title },
        { name: 'twitter:title', content: this.title },
        { name: 'twitter:description', content: this.description },
        { name: 'twitter:image', content: '/img/guillermo-card.png' },
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
