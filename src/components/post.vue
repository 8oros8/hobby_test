<template>
  <div class="postItem">
    <div class="postItem__wrapper">
      <header class="postItem__title">{{postData.title}}</header>
      <div class="postItem__author">Post by user: {{postData.userId}}</div>
      <textarea class="postItem__body"
                v-bind:class="{ postItem__body_active: !disabled }"
                v-model="postData.body"
                :disabled="disabled"
                @blur="disabled = true"></textarea>
    </div>
    <custom-button :click-descriptor="'Редактировать'"
                   @click="disabled = !disabled"></custom-button>
    <custom-button :click-descriptor="'Удалить'"
                   @click="$emit('deletePost', postData)"></custom-button>
  </div>
</template>

<script>
import CustomButton from "./customButton.vue";

export default {
  name: "post",
  components: {CustomButton},
  data() {
    return {
      disabled: true,
    }
  },
  watch: {
    disabled: function () {
      this.savePosts()
    }
  },
  props: {
    postData: {},
    savePosts: Function,
  },
}
</script>

<style scoped>

.postItem {
  display: flex;
  width: 80%;
}

.postItem__wrapper {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: space-between;
  width: 600px;
  height: 150px;
  border: solid #061F70;
  border-radius: 10px;
  margin-bottom: 10px;
  margin-right: 10px;
  padding: 8px;
  background-color: whitesmoke;
}

.postItem__title {
  font-size: 20px;
  font-weight: 700;
}

.postItem__body {
  display: flex;
  font-size: 16px;
  font-weight: 400;
  background-color: transparent;
  margin-top: 10px;
  height: 80%;
  width: 100%;
  border: none;
  border-radius: 10px;
  color: black;
  resize: none;
}
.postItem__body_active {
  background-color: yellow;
}

.postItem__author {
  font-size: 16px;
  font-weight: 400;
}


</style>