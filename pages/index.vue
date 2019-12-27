<template>
  <div id="home">
    <div class="list-news-wrap">
      <ul class="list-news">
        <ListPostItem v-for="post in posts" :key="post.id" :post="post" />
      </ul>
      <Pagination :currentPage="currentPage" :totalPages="totalPages" />
    </div>
  </div>
</template>

<script>
import ListPostItem from '@/components/ListPostItem.vue'
import Pagination from '@/components/Pagination.vue'
export default {
  name: 'Home',
  watchQuery: ['page'],
  key: (to) => to.fullPath,
  components: {
    ListPostItem,
    Pagination
  },
  data() {
    return {
      posts: [],
      currentPage: 1,
      totalPages: 0,
      totalPosts: 0,
      title: 'Home'
    }
  },
  asyncData({ $axios, query, error }) {
    const page = +query.page || 1
    return $axios
      .get(
        `https://techtalk.vn/wp-json/wp/v2/posts?_embed&page=${page}&per_page=15`
      )
      .then((res) => {
        return {
          posts: res.data,
          totalPosts: +res.headers['x-wp-total'],
          totalPages: +res.headers['x-wp-totalpages'],
          currentPage: +page,
          title:
            'Result page ' +
            +page +
            ' of ' +
            +res.headers['x-wp-totalpages'] +
            ' for Blog'
        }
      })
      .catch((e) => {
        console.log('Request failed', e)
        error({ statusCode: 404, message: e })
      })
  },
  transition(to, from) {
    if (!from) {
      return 'page'
    }
    return +to.query.page < +from.query.page ? 'slide-right' : 'slide-left'
  },
  head() {
    return {
      title: this.title,
      meta: []
    }
  }
}
</script>

<style lang="scss" scoped>
.list-news-wrap {
  .list-news {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(320px, auto));
    grid-gap: 1rem;
    padding: 0;
    margin: 0;
  }
}
</style>
