<template>
  <div class="app">
    <h1>Post page</h1>
    <div class="app_btns">
      <my-button @click="showDialog">Create post</my-button>
      <my-select
          v-model="selectedSort"
          :options="sortOptions"
      />
    </div>

    <my-dialog v-model:show="dialogVisible">
      <post-form :name=23 v-on:create="createPost"/>
    </my-dialog>

    <post-list
        :posts="posts"
        @remove="removePost"
        v-if="!isPostLoading"
    />
    <div v-else>
      <p style="color: red">loading...</p>
    </div>
  </div>
</template>

<script>


import PostForm from "@/components/PostForm"
import PostList from "@/components/PostList"
import MyDialog from "@/components/UI/MyDialog"
import MyButton from "@/components/UI/MyButton"
import axios from 'axios'
import MySelect from "@/components/UI/MySelect";

export default {
  components: {MySelect, MyButton, MyDialog, PostList, PostForm},
  data() {
    return {
      posts: [],
      dialogVisible: false,
      isPostLoading: false,
      selectedSort: '',
      sortOptions: [
        {value:'title', name: 'By name'},
        {value:'body', name: 'By description'},
      ]
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post)
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },
    async fetchPosts() {
      try {
        this.isPostLoading = true
        const response = await axios.get("https://jsonplaceholder.typicode.com/posts?_limit=10")
        this.posts = response.data
      } catch (e) {
        alert(e)
      } finally {
        this.isPostLoading = false
      }
    },
  },
  mounted() {
    this.fetchPosts()
  },
  watch:{
    selectedSort(newValue){
      this.posts.sort((post1, post2)=> {
        return post1[newValue]?.localeCompare(post2[newValue])
      })
    },



  }

}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
}

.app_btns{
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}
</style>