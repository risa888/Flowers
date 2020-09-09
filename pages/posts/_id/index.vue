<template>
  <main class="container my-5">
    <div class="row">
      <div class="col-12 text-center my-3">
        <h2 class="mb-3 display-4 text-uppercase">
          {{ post.title }}
        </h2>
      </div>
      <div class="col-md-6 mb-4">
        <img
          :src="post.picture"
          class="img-fluid"
          style="width: 400px; border-radius: 10px; box-shadow: 0 1rem 1rem rgba(0,0,0,.7);"
          alt
        >
      </div>
    </div>
  </main>
</template>
<script>
export default {
  head () {
    return {
      title: '水草詳細ページ'
    }
  },
  data () {
    return {
      post: {
        pub_date: '',
        title: '',
        picture: ''
      }
    }
  },
  async asyncData ({ $axios, params }) {
    try {
      const post = await $axios.$get(`/posts/${params.id}`)
      return { post }
    } catch (e) {
      return { post: [] }
    }
  }
}
</script>
<style scoped>
</style>
