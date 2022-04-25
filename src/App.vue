<script>

import Post from "./components/post.vue";
import CustomButton from "./components/customButton.vue";
import { toRaw } from "vue"


export default {
  name: 'App',
  components: {CustomButton, Post},
  data() {
    return {
      postsData: [],
      sortingOption: '',
      postCreation: false,
      postInCreation: {},
      nextId: 101,
    }
  },
  mounted() {
    if (localStorage.postsData) {
      try {
        this.postsData = JSON.parse(localStorage.getItem('postsData'))
      }
      catch (e) {
        localStorage.removeItem('postsData')
      }
    }
  },
  computed: {
    displayedData() {
      if (this.sortingOption === '') {
        return this.postsData
      }
      return this.postsData.filter((item) => item.userId == this.sortingOption)
    },
    users() {
      let userIdData = []
      this.postsData.forEach((item) => {
        userIdData.push(item.userId)
      })
      return new Set(userIdData)
    }
  },
  methods: {
    fetchPosts() {
      fetch('https://jsonplaceholder.typicode.com/posts/')
          .then((response) => response.json())
          .then((json) =>
              this.postsData = json)
          .then((json) => {
            let parsed = JSON.stringify(json)
            localStorage.setItem('postsData', parsed)
            return json
          })
    },
    savePosts() {
      let parsed = JSON.stringify(this.postsData)
      localStorage.setItem('postsData', parsed)
    },
    deletePost(post) {
      if (confirm('Вы уверены?')) {
        this.postsData.splice(this.postsData.findIndex(element => element === post), 1)
      }
      this.savePosts()
    },
    addPost() {
      if ((this.postInCreation.title !== undefined)
          && (this.postInCreation.userId !== undefined)
          && (this.postInCreation.body !== undefined)) {
        this.postInCreation.id = this.nextId
        this.nextId++
        this.postsData.unshift(this.postInCreation)
        this.postInCreation = {}
        this.savePosts()
      }
      else {alert('Пожалуйста, заполните все поля')}
    },
  }
}
</script>

<template>
  <div class="appWrapper">
    <div class="buttonsWrapper">
      <custom-button :click-descriptor="'Загрузить'"
                     @click="fetchPosts">
      </custom-button>
      <custom-button :click-descriptor="'Добавить'"
                     @click="postCreation = true">
      </custom-button>
      <select class="customSelect" v-model="sortingOption">
        <option selected></option>
        <option v-for="userId in users">
          {{ userId }}
        </option>
      </select>
    </div>
    <div class="postCreation"
         v-if="postCreation">
      <input placeholder="Post title:" v-model="postInCreation.title">
      <input placeholder="User id:" v-model="postInCreation.userId">
      <textarea class="postCreation__postWindow" placeholder="Type your post here:" v-model="postInCreation.body"></textarea>
      <custom-button class="postCreation__apply"
                     :click-descriptor="'Применить'"
                     @click="addPost"></custom-button>
    </div>
    <post v-for="item in this.displayedData"
          :postData="item"
          v-on:deletePost="deletePost($event)"
          :savePosts="savePosts">
    </post>
  </div>
</template>

<style>
@import './assets/base.css';

.appWrapper {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: space-between;
  padding: 10px;
}
.buttonsWrapper {
  display: flex;
  justify-content: space-between;
}
.customSelect {
  display: flex;
  text-align: center;
  padding: 2px;
  cursor: pointer;
  width: 160px;
  height: 50px;
  background-color: lightblue;
  border: solid cadetblue 2px;
  border-radius: 10px;
  margin-bottom: 10px;
  margin-right: 10px;
}
.postCreation {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 500px;
  height: 300px;
  border: solid #061F70;
  border-radius: 10px;
  margin-bottom: 10px;
  margin-right: 10px;
  padding: 18px;
  background-color: whitesmoke;
}
.postCreation > input {
  margin-bottom: 8px;
  width: 100%;
}
.postCreation__postWindow {
  height: 200px;
  width: 100%;
  resize: none;
}
.postCreation__apply {
  margin-top: 10px;
}
</style>
