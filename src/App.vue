<template>
  <div id="app">
    <Game />
  </div>
</template>

<script>
// Importing component
import Game from "./components/Game.vue";

export default {
  name: "App",
  components: {
    Game,
  },
};
</script>

<style>
body {
  background-image: url("https://w.wallhaven.cc/full/q6/wallhaven-q6pmxr.png");
}
</style>
