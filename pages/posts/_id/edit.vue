<template>
  <main class="container my-5">
    <div class="row">
      <div class="col-12 text-center my-3">
        <h2 class="mb-3 display-4 text-uppercase">
          {{ post.title }}
        </h2>
      </div>
      <div class="col-md-6 mb-4">
        <img v-if="!preview" :src="post.picture" class="img-fluid" style="width: 400px border-radius: 10px box-shadow: 0 1rem 1rem rgba(0,0,0,.7)">
        <img v-else :src="preview" class="img-fluid" style="width: 400px border-radius: 10px box-shadow: 0 1rem 1rem rgba(0,0,0,.7)">
      </div>
      <div class="col-md-4">
        <form @submit.prevent="submitWaterPlant">
          <div class="form-group">
            <label for>水草の名称</label>
            <input v-model="post.title" type="text" class="form-control">
          </div>
          <div class="form-group">
            <label for>水草の写真</label>
            <input @change="onFileChange" type="file">
          </div>
          <button type="submit" class="btn btn-success">
            更新
          </button>
        </form>
      </div>
    </div>
  </main>
</template>
<script>
export default {
  head () {
    return {
      title: '水草編集ページ'
    }
  },
  data () {
    return {
      post: {
        pub_date: '',
        title: '',
        picture: ''
      },
      preview: ''
    }
  },
  async asyncData ({ $axios, params }) {
    try {
      const post = await $axios.$get(`/posts/${params.id}`)
      return { post }
    } catch (e) {
      return { post: [] }
    }
  },
  methods: {
    onFileChange (e) {
      const files = e.target.files || e.dataTransfer.files
      if (!files.length) {
        return
      }
      this.post.picture = files[0]
      this.createImage(files[0])
    },
    createImage (file) {
      const reader = new FileReader()
      const vm = this
      reader.onload = (e) => {
        vm.preview = e.target.result
      }
      reader.readAsDataURL(file)
    },
    async submitWaterPlant () {
      const editedWaterPlant = this.post
      if (editedWaterPlant.picture.includes('http://') !== -1) {
        delete editedWaterPlant.picture
      }
      const config = {
        headers: { 'content-type': 'multipart/form-data' }
      }
      const formData = new FormData()
      for (const data in editedWaterPlant) {
        formData.append(data, editedWaterPlant[data])
      }
      try {
        const response = await this.$axios.$patch(`/posts/${editedWaterPlant.id}/`, formData, config)
        this.$router.push('/posts/')
        console.log(response)
      } catch (e) {
        console.log(e)
      }
    }
  }
}
</script>

<style>
</style>
