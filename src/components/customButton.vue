<template>
  <button class="customButton">
    {{clickDescriptor}}
  </button>
</template>

<script>
export default {
  name: "customButton",
  props: {
    clickDescriptor: String,
  }
}
</script>

<style>
.customButton {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 2px;
  cursor: pointer;
  width: 120px;
  height: 50px;
  background-color: lightblue;
  border: solid cadetblue 2px;
  border-radius: 10px;
  margin-bottom: 10px;
  margin-right: 10px;
}
.customButton:hover {
  background-color: mediumturquoise;
}
.customButton:active {
  background-color: cadetblue;
}
</style>