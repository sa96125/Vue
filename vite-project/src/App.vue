<template>
  <HelloWorld />
</template>

<script lang="ts">
import * as vue from "vue";
import HelloWorld from "./components/HelloWorld.vue";

export default vue.defineComponent({
  name: "App",
  components: {
    HelloWorld,
  },
});
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Noto+Serif+KR&display=swap");

body {
  margin: 0%;
}
#app {
  font-family: "Noto Serif KR", serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
