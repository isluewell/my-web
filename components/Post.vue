<template>
  <div>
    <div class="max-w-5xl mx-auto">
      <!-- header -->
      <nav-blog />
      <!-- header ends here -->

      <main class="mt-10">
        <div
          class="mb-4 md:mb-0 w-full max-w-screen-md mx-auto relative"
          style="height: 24em"
        >
          <div
            class="absolute left-0 bottom-0 w-full h-full z-10"
            style="
              background-image: linear-gradient(
                180deg,
                transparent,
                rgba(0, 0, 0, 0.7)
              );
            "
          ></div>
          <img
            :src="article.img"
            :alt="article.alt"
            class="absolute left-0 top-0 w-full h-full z-0 object-cover"
          />
          <div class="p-4 absolute bottom-0 left-0 z-20">
            <nuxt-link
              v-for="(tag, id) in article.tags"
              :key="id"
              :v-if="article.slug"
              :to="
                $store.state.data.lang === 'es'
                  ? `/blog/${article.slug}`
                  : `/en/blog/${article.slug}`
              "
              class="px-4 bg-black text-gray-200 inline-flex items-center justify-center mb-2 mr-1"
            >
              {{ tags[tag].name }}
            </nuxt-link>
            <h2 class="text-4xl font-semibold text-gray-100 leading-tight">
              {{ article.title }}
            </h2>
            <div class="flex mt-3">
              <!-- <img
                src="https://randomuser.me/api/portraits/men/97.jpg"
                class="h-10 w-10 rounded-full mr-2 object-cover"
              /> -->
              <div>
                <!-- <p class="font-semibold text-gray-200 text-sm">Mike Sullivan</p> -->
                <p class="font-semibold text-white text-xs">
                  {{ formatDate(article.updatedAt) }}
                </p>
                <!-- <nuxt-link
                  v-if="article.Anotherlanguage"
                  :to="
                    $store.state.data.lang === 'es'
                      ? `/en/blog/${article.Anotherlanguage}`
                      : `/blog/${article.Anotherlanguage}`
                  "
                > -->
                <p
                  v-if="article.Anotherlanguage"
                  class="cursor-pointer font-semibold text-xs px-4 bg-gray-900 text-gray-200 inline-flex items-center justify-center"
                  @click="redirectBlog"
                >
                  {{ texts.text1 }}
                </p>
                <!-- </nuxt-link> -->
              </div>
            </div>
          </div>
        </div>

        <div
          class="px-4 lg:px-0 mt-12 text-gray-700 max-w-2xl mx-auto text-lg leading-relaxed"
        >
          <!-- content from markdown -->
          <nuxt-content :document="article" />
          <!-- prevNext component -->
          <PrevNext :prev="prev" :next="next" class="mt-8" />
        </div>
      </main>
      <!-- main ends here -->

      <!-- footer -->
    </div>

    <pie />
  </div>
</template>
<script>
export default {
  props: {
    article: {
      type: Object,
      required: true,
      default: () => {}
    },
    tags: {
      type: Object,
      required: true,
      default: () => {}
    },
    prev: {
      type: Object,
      default: () => null
    },
    next: {
      type: Object,
      default: () => null
    }
  },
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
    this.texts = texts.article
  },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('en', options)
    },
    redirectBlog() {
      if (this.$store.state.data.lang === 'es') {
        this.$store.commit('data/SET_LANG', 'en')
        this.$router.push({
          path: `/en/blog/${this.article.Anotherlanguage}`
        })
      } else {
        this.$store.commit('data/SET_LANG', 'es')
        this.$router.push({
          path: `/blog/${this.article.Anotherlanguage}`
        })
      }
    }
  }
}
</script>
<style>
.nuxt-content p {
  margin-bottom: 20px;
}
.nuxt-content h2 {
  font-weight: bold;
  font-size: 28px;
}
.nuxt-content h3 {
  font-weight: bold;
  font-size: 22px;
}
.icon.icon-link {
  background-image: url('~assets/svg/icon-hashtag.svg');
  display: inline-block;
  width: 20px;
  height: 20px;
  background-size: 20px 20px;
}
</style>
