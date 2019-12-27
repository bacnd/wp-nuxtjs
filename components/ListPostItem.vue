<template>
  <li class="news-item">
    <nuxt-link
      :to="{
        name: 'detail-slug',
        params: { post: post, slug: post.slug }
      }"
      ><img :src="getThumb" :alt="post.title.rendered"
    /></nuxt-link>
    <span><i>—</i> {{ formatDate }} <i>—</i></span>
    <h3>
      <nuxt-link
        :to="{
          name: 'detail-slug',
          params: { post: post, slug: post.slug }
        }"
        v-html="post.title.rendered"
      />
    </h3>
  </li>
</template>

<script>
export default {
  name: 'ListPostItem',
  props: {
    post: {
      type: Object,
      default: () => {}
    }
  },
  computed: {
    getThumb() {
      if (
        this.post._embedded['wp:featuredmedia'] &&
        this.post._embedded['wp:featuredmedia'][0].media_details &&
        this.post._embedded['wp:featuredmedia'][0].media_details.sizes &&
        Object.keys(
          this.post._embedded['wp:featuredmedia'][0].media_details.sizes
        ).length > 7
      ) {
        return this.post._embedded['wp:featuredmedia'][0].media_details.sizes
          .td_324x160.source_url
      }
      return this.post._embedded['wp:featuredmedia'][0].media_details.sizes.full
        .source_url
    },
    formatDate() {
      const date = this.post.date ? new Date(this.post.date) : new Date()
      return (
        date.getDay() + '/' + (date.getMonth() + 1) + '/' + date.getFullYear()
      )
    }
  }
}
</script>

<style lang="scss" scoped>
li {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-orient: vertical;
  -webkit-box-direction: normal;
  -ms-flex-direction: column;
  flex-direction: column;
  background: #fafafa;
  border: 2px solid #ededed;
  padding: 2rem 1rem;
  text-align: center;
  a {
    display: block;
    text-decoration: none;
    -webkit-transition: color 0.15s ease;
    transition: color 0.15s ease;
    cursor: pointer;
  }
  img {
    max-width: 100%;
  }
  span {
    font-size: 0.75rem;
    color: #999;
    margin: 1rem 0 0.5rem;
  }
  h3 {
    margin: 0 0.25rem 0 1rem;
    a {
      color: inherit;
    }
  }
}
</style>
