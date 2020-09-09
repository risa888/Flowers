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
          v-if="preview"
          :src="preview"
          class="img-fluid"
          style="width: 400px border-radius: 10px box-shadow: 0 1rem 1rem rgba(0,0,0,.7)"
          alt
        >
        <img
          v-else
          src="@/static/images/banner.jpg"
          class="img-fluid"
          style="width: 400px border-radius: 10px box-shadow: 0 1rem 1rem rgba(0,0,0,.7)"
        >
      </div>
      <div class="col-md-4">
        <form @submit.prevent="submitWaterPlant">
          <div class="form-group">
            <label for>植える位置</label>
            <input v-model="post.title" type="text" class="form-control">
          </div>
          <div class="form-group">
            <label for>水草の写真</label>
            <input @change="onFileChange" type="file" name="file">
          </div>
          <button type="submit" class="btn btn-primary">
            登録
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
      title: '水草追加'
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
      const config = {
        headers: { 'content-type': 'multipart/form-data' }
      }
      const formData = new FormData()
      for (const data in this.post) {
        formData.append(data, this.post[data])
      }
      try {
        const response = await this.$axios.$post('/posts/', formData, config) // eslint-disable-line
        this.$router.push('/posts/')
      } catch (e) {
        console.log(e)
      }
    }
  }
}
</script>
<style scoped>
</style>
