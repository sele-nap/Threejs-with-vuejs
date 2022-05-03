<template>
  <div id="myThreeJsCanvas"></div>
</template>
<script>
import * as THREE from "three";

export default {
  name: "CubeMultifaces",
  data() {
    return {
      scene: undefined,
      camera: undefined,
      renderer: undefined,
    };
  },
  methods: {
    init() {
      //initialize scene and background
      const BACKGROUND_COLOR = "pink";
      this.scene = new THREE.Scene();
      this.scene.background = new THREE.Color(BACKGROUND_COLOR);

      const canvas = document.querySelector("#c");
      const renderer = new THREE.WebGLRenderer({ canvas });
      const cubes = [];

      const loader = new THREE.TextureLoader();
      let materials = [
        new THREE.MeshBasicMaterial({
          map: loader.load("/assets/jupiter.jpeg"),
        }),
        new THREE.MeshBasicMaterial({
          map: loader.load("/assets/mercury.jpeg"),
        }),
        new THREE.MeshBasicMaterial({
          map: loader.load("/assets/neptune.jpeg"),
        }),
        new THREE.MeshBasicMaterial({
          map: loader.load("/assets/saturn.jpeg"),
        }),
        new THREE.MeshBasicMaterial({
          map: loader.load("/assets/uranus.jpeg"),
        }),
        new THREE.MeshBasicMaterial({
          map: loader.load("/assets/venus.jpeg"),
        }),
      ];

      //cube
      const boxWidth = 1;
      const boxHeight = 1;
      const boxDepth = 1;
      const geometry = new THREE.BoxBufferGeometry(
        boxWidth,
        boxHeight,
        boxDepth
      );

      const cube = new THREE.Mesh(geometry, materials);
      this.scene.add(cube);
      cubes.push(cube);

      function resizeRendererToDisplaySize(renderer) {
        const canvas = renderer.domElement;
        const width = canvas.clientWidth;
        const height = canvas.clientHeight;
        const needResize = canvas.width !== width || canvas.height !== height;
        if (needResize) {
          renderer.setSize(width, height, false);
        }
        return needResize;
      }

      function render(time) {
        time *= 0.001;

        if (resizeRendererToDisplaySize(renderer)) {
          const canvas = renderer.domElement;
          this.camera.aspect = canvas.clientWidth / canvas.clientHeight;
          this.camera.updateProjectionMatrix();
        }

        cubes.forEach((cube, ndx) => {
          const speed = 0.2 + ndx * 0.1;
          const rot = time * speed;
          cube.rotation.x = rot;
          cube.rotation.y = rot;
        });

        this.renderer.render(this.scene, this.camera);

        requestAnimationFrame(render);
      }

      requestAnimationFrame(render);
    },
  },
  mounted() {
    this.init();
  },
};
</script>