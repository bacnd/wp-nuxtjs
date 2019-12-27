<template>
  <nav id="pagination">
    <ul v-if="totalPages != 1" class="pagination page-guides">
      <li>
        <nuxt-link
          v-if="currentPage != 1"
          :to="{ name: '', query: { page: 1 } }"
          >First</nuxt-link
        >
        <span v-else>First</span>
      </li>
      <li>
        <nuxt-link
          v-if="prevpage != null"
          :to="{ name: '', query: { page: prevpage } }"
          >&laquo; Forward</nuxt-link
        >
        <span v-else>&laquo; Forward</span>
      </li>
      <template v-for="num in pageNumbers">
        <li v-if="num !== null" :key="num">
          <nuxt-link
            v-if="num !== currentPage"
            :to="{ name: '', query: { page: num } }"
            >{{ num }}</nuxt-link
          >
          <span v-else class="active">{{ num }}</span>
        </li>
      </template>
      <li>
        <nuxt-link
          v-if="nextpage != null && currentPage != totalPages"
          :to="{ name: '', query: { page: nextpage } }"
          >Next &raquo;</nuxt-link
        >
        <span v-else>Next &raquo;</span>
      </li>
      <li>
        <nuxt-link
          v-if="currentPage != totalPages"
          :to="{ name: '', query: { page: totalPages } }"
          >Last</nuxt-link
        >
        <span v-else>Last</span>
      </li>
    </ul>
  </nav>
</template>

<script>
export default {
  name: 'Pagination',
  props: {
    currentPage: {
      type: Number,
      default: 1
    },
    totalPages: {
      type: Number,
      default: 1
    }
  },
  data() {
    return {
      nextpage: null,
      prevpage: null,
      pageNumbers: [],
      pageNumberCount: 0
    }
  },
  mounted() {
    this.setPageNumbers()
  },
  methods: {
    setPages(currentPage, totalPages) {
      this.prevpage = currentPage > 1 ? currentPage - 1 : null
      if (!totalPages) {
        this.nextpage = this.currentPage ? parseInt(this.currentPage) + 1 : 2
      } else {
        this.nextpage =
          currentPage < totalPages ? parseInt(currentPage) + 1 : null
      }
      for (let i = 0; i < 7; i++) {
        const _p = parseInt(currentPage) - 3 + i
        if (_p > 0 && _p <= totalPages) {
          this.pageNumbers.push(_p)
          this.pageNumberCount++
        } else this.pageNumbers.push(null)
      }
    },
    setPageNumbers() {
      const _currentPage = this.currentPage ? this.currentPage : 1
      this.currentPage = _currentPage
      this.setPages(_currentPage, this.totalPages)
    }
  }
}
</script>

<style lang="scss" scoped>
.pagination {
  margin: 1rem 0 0;
  padding: 0;
  padding-top: 20px;
  list-style: none;
  display: -webkit-box;
  display: flex;
  flex-wrap: wrap;
  -webkit-box-pack: center;
  justify-content: center;
  li {
    .active {
      background-color: #4dba87;
      color: white;
      border: 1px solid #4dba87;
    }
    a,
    span {
      color: black;
      float: left;
      padding: 6px 14px;
      text-decoration: none;
      -webkit-transition: background-color 0.3s;
      transition: background-color 0.3s;
      border: 1px solid #ddd;
    }
    a {
      &:hover {
        background-color: #ddd;
      }
    }
  }
}
</style>
