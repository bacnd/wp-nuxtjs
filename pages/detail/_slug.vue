<template>
  <div v-if="isValidPost()" class="news-detail">
    <h1 v-html="post.title.rendered" class="title"></h1>
    <div v-html="post.content.rendered" class="news-content"></div>
  </div>
</template>

<script>
export default {
  name: 'Detail',
  data() {
    return {
      post: {}
    }
  },
  async asyncData({ $axios, params, error }) {
    console.log('asyncData post single', params)
    if (!params.post) {
      const slug = params.slug
      try {
        const res = await $axios.get(
          `https://techtalk.vn/wp-json/wp/v2/posts?_embed&slug=${slug}`
        )
        if (!res.data.length) {
          error({ statusCode: 404, message: 'Post not found!' })
        }
        return { post: res.data.length ? res.data[0] : [] }
      } catch (error) {
        console.log('parsing failed', error)
        error({ statusCode: 404, message: error })
      }
    } else {
      return { post: params.post }
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.$nuxt.$loading.start()
      setTimeout(() => this.$nuxt.$loading.finish(), 500)
    })
    console.log(this.post)
  },
  methods: {
    isValidPost() {
      return Object.entries(this.post).length !== 0
    }
  },
  head() {
    const post = this.post
    return this.isValidPost()
      ? {
          title: post.title.rendered,
          meta: [
            {
              hid: 'description',
              name: 'description',
              content: post.excerpt.rendered.replace(/<\/?[^>]+(>|$)/g, '')
            },
            {
              hid: 'og:title',
              property: 'og:title',
              content: post.title.rendered
            },
            {
              hid: 'og:site_name',
              property: 'og:site_name',
              content: 'Minimal Blog Nuxt JS + Wordpress API'
            },
            {
              hid: 'og:url',
              property: 'og:url',
              content: process.env.baseUrl + this.$nuxt.$route.fullPath
            },
            {
              hid: 'og:description',
              property: 'og:description',
              content: post.excerpt.rendered.replace(/<\/?[^>]+(>|$)/g, '')
            },
            {
              hid: 'og:type',
              property: 'og:type',
              content: 'article'
            },
            {
              hid: 'og:image',
              property: 'og:image',
              content: post._embedded['wp:featuredmedia'][0].source_url
            }
          ]
        }
      : {}
  }
}
</script>

<style lang="scss" scoped>
.news-content {
  h1,
  h2,
  h3,
  h4,
  h5 {
    margin: auto;
  }
}
.news-content {
  position: relative;
  margin-top: 30px;
  embed,
  iframe,
  object {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    max-width: 100%;
  }
}
.crayon-syntax {
  width: 100%;
  overflow: hidden;
  .crayon-main {
    background-color: #f6f8fa;
    border-radius: 3px;
    font-size: 85%;
    line-height: 1.45;
    overflow: auto;
    padding: 16px;
    .crayon-nums {
      display: none;
    }
  }
}
</style>
