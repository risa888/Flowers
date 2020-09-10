<template>
  <main class="container mt-5">
    <div class="row">
      <div class="col-12 text-right mb-4">
        <div class="d-flex justify-content-between">
          <h3>Life with flowers</h3>
          <nuxt-link to="/posts/add" class="btn btn-info">
            写真を投稿
          </nuxt-link>
        </div>
      </div>
      <template v-for="post in posts">
        <div :key="post.id" class="col-lg-3 col-md-4 col-sm-6 mb-4">
          <water-plant-card :onDelete="deleteWaterPlant" :post="post" />
        </div>
      </template>
    </div>
  </main>
</template>
<script>

import WaterPlantCard from '~/components/WaterPlantCard.vue'

export default {
  head () {
    return {
      title: '水草一覧'
    }
  },
  components: {
    WaterPlantCard
  },
  data () {
    return {
      posts: []
    }
  },
  async asyncData ({ $axios, params }) {
    try {
      const posts = await $axios.$get(`/posts/`)
      return { posts }
    } catch (e) {
      return { posts: [] }
    }
  },
  methods: {
    async deleteWaterPlant (post_id) { // eslint-disable-line
      try {
        await this.$axios.$delete(`/posts/${post_id}/`) // eslint-disable-line
        const newPosts = await this.$axios.$get('/posts/')
        this.post = newPosts
      } catch (e) {
        console.log(e)
      }
    }
  }
}
</script>
<style scoped>
</style>
