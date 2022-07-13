<template>
  <div>
    <form @submit.prevent>
      <h4>Post creation</h4>
      <my-input
          v-model="post.title"
          placeholder="Title"
      />

      <my-input
          v-model="post.body"
          placeholder="Description"
      />
      <my-button @click="createPost" style="margin-top: 15px; align-self: flex-end">Create</my-button>

    </form>
  </div>
</template>

<script>


export default {
  data() {
    return {
      post: {
        title: '',
        body: ''
      }
    }
  },
  methods: {
    createPost() {
      this.post.id = new Date()
      this.$emit('create', this.post)
      this._clear()
    },
    _clear() {
      this.post = {
        title: '',
        body: ''
      }
    }

  }
}
</script>

<style scoped>


form {
  margin-bottom: 15px;
  display: flex;
  flex-direction: column;
}


</style>