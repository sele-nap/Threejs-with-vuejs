<template>
  <div id="app">
    <test-scene />
  </div>
</template>

<script>
import TestScene from "./components/TestScene.vue";
// import * as THREE from "three";

export default {
  name: "App",
  components: {
    TestScene,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
