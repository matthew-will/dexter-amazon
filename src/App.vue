<template>
  <div id="app">
    <Video />
  </div>
</template>

<script>
import Video from "./components/Video.vue";

export default {
  name: "App",
  components: {
    Video,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
body {
  margin: 0;
}
</style>
