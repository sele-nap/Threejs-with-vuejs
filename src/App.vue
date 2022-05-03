<template>
  <div id="app">
    <test-scene />
    <cube-multifaces />
  </div>
</template>

<script>
import TestScene from "./components/TestScene.vue";
import CubeMultifaces from "./components/CubeMultifaces.vue";
// import * as THREE from "three";

export default {
  name: "App",
  components: {
    CubeMultifaces,
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
