<template>
  <div id="myThreeJsCanvas"></div>
</template>
<script>
import * as THREE from "three";
// import SceneInit from "../lib/SceneInit";
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";

export default {
  name: "TestScene",
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
      const BACKGROUND_COLOR = "lightblue";
      this.scene = new THREE.Scene();
      this.scene.background = new THREE.Color(BACKGROUND_COLOR);

      //const TextureLoader = new THREE.TextureLoader();
      const marsTexture = new THREE.TextureLoader().load("./assets/mars.jpeg");

      //test sphere
      const ge3 = new THREE.SphereGeometry(4);
      const me3 = new THREE.MeshStandardMaterial({
        map: marsTexture,
      });
      const sphereMe3 = new THREE.Mesh(ge3, me3);
      sphereMe3.position.x = -200;
      sphereMe3.position.y = 200;
      sphereMe3.scale.set(10, 10, 10);
      this.scene.add(sphereMe3);

      // Adding gtlf model
      let loader = new GLTFLoader();
      loader.load("/3DModel/Cat/scene.gltf", (gltfScene) => {
        // gltfScene.scene.rotation.y = Math.PI / 8;
        // gltfScene.scene.position.y = 3;
        gltfScene.scene.scale.set(1, 1, 1);
        this.scene.add(gltfScene.scene);
        console.log(gltfScene.scenes);
      });

      //initialize renderer
      this.renderer = new THREE.WebGLRenderer({
        container,
        antialias: true,
        alpha: true,
      });
      this.renderer.setPixelRatio(window.devicePixelRatio);
      this.renderer.outputEncoding = THREE.sRGBEncoding;

      //get container
      const container = document.getElementById("myThreeJsCanvas");
      this.renderer.setSize(window.innerWidth, window.innerHeight);
      this.renderer.shadowMap.enabled = true;
      container.appendChild(this.renderer.domElement);

      //Adding camera
      this.camera = new THREE.PerspectiveCamera(
        45,
        container.offsetWidth / container.offsetHeight,
        1,
        1000
      );

      //Floor : Adding floor
      const floorGeometry = new THREE.PlaneGeometry(5000, 5000, 1, 1);
      const floorMaterial = new THREE.MeshPhongMaterial({
        color: 0xff311b92,
        shininess: 0,
      });
      var floor = new THREE.Mesh(floorGeometry, floorMaterial);
      floor.rotation.x = -0.5 * Math.PI;
      floor.receiveShadow = true;
      floor.position.y = -10;
      this.scene.add(floor);
      this.camera.position.set(100, 100, 100);

      //Adding controls
      const controls = new OrbitControls(this.camera, this.renderer.domElement);
      //controls.enablePan = false;
      controls.target.set(0, 0, 0);
      controls.addEventListener("change", this.renderScene); // use if there is no animation loop
      window.addEventListener("resize", () => this.onWindowResize(), false);

      //Adding light
      const directionalLight = new THREE.DirectionalLight(0xffffff, 2);
      directionalLight.position.set(0, 1, 0);
      directionalLight.castShadow = true;
      this.scene.add(directionalLight);
      const light = new THREE.PointLight(0xffffcc, 0.5);
      light.position.set(0, 600, 1000);
      this.scene.add(light);
      const light2 = new THREE.PointLight(0xe6f7ff, 0.5);
      light2.position.set(1000, 200, 0);
      this.scene.add(light2);
      const light3 = new THREE.PointLight(0xfff2e6, 0.5);
      light3.position.set(0, 200, -1000);
      this.scene.add(light3);
      const light4 = new THREE.PointLight(0xc4c400, 0.5);
      light4.position.set(-1000, 600, 1000);
      this.scene.add(light4);
      const hemiLight = new THREE.HemisphereLight(0xffffff, 0xffffff, 0.2);
      hemiLight.position.set(0, 50, 0);

      // Ajoute une lumière hémisphérique à la scène
      this.scene.add(hemiLight);

      //texture

      // const spaceTexture = new THREE.TextureLoader().load(
      //   "../assets/mars.jpeg"
      // );

      // //Add an object
      // const groundGeometry = new THREE.SphereGeometry(4);
      // const groundMaterial = new THREE.MeshStandardMaterial({
      //   map: spaceTexture,
      // });
      // const groundMesh = new THREE.Mesh(groundGeometry, groundMaterial);
      // groundMesh.receiveShadow = true;
      // groundMesh.position.y = -2;
      // this.scene.add(groundMesh);
      this.animate();
    },

    renderScene() {
      this.renderer.render(this.scene, this.camera);
    },
    animate() {
      // NOTE: Window is implied.
      // requestAnimationFrame(this.animate.bind(this));
      window.requestAnimationFrame(this.animate.bind(this));
      this.renderScene();
    },
    onWindowResize() {
      this.camera.aspect = window.innerWidth / window.innerHeight;
      this.camera.updateProjectionMatrix();
      this.renderer.setSize(window.innerWidth, window.innerHeight);
    },
  },
  mounted() {
    this.init();
  },
};
</script>