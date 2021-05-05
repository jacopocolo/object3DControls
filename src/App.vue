<template>
  <div></div>
</template>

<script>
import * as THREE from "three";
import { ObjectControls } from "./components/OrbitControlsForObject.js";
export let scene, camera, renderer, cube;
let controls;

export default {
  name: "App",
  components: {},
  methods: {
    init: function () {
      scene = new THREE.Scene();
      camera = new THREE.PerspectiveCamera(
        75,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );
      renderer = new THREE.WebGLRenderer();
      renderer.setSize(window.innerWidth, window.innerHeight);
      document.body.appendChild(renderer.domElement);

      var axesHelper = new THREE.AxesHelper();
      axesHelper.applyMatrix4(new THREE.Matrix4().makeScale(5, 5, 5));
      axesHelper.layers.set(0);
      axesHelper.material.fog = false;
      scene.add(axesHelper);

      const geometry = new THREE.BoxGeometry();
      const material = new THREE.MeshBasicMaterial({
        color: 0x00ff00,
        transparent: true,
        opacity: 0.8,
        side: THREE.DoubleSide,
      });
      cube = new THREE.Mesh(geometry, material);
      cube.position.set(1, 1, 1);
      scene.add(cube);

      controls = new ObjectControls(cube, camera, renderer.domElement);
      controls.showTouches = true;
      controls.addEventListener(
        "change",
        () => {
          controls.update();
          // camera.lookAt(cube.position.x, cube.position.y, cube.position.z);
          renderer.render(scene, camera);
        },
        false
      );
      controls.update();
      camera.position.set(3, 2, 5);
      // camera.lookAt(0, 0, 0);
      renderer.render(scene, camera);
    },
    animate() {
      requestAnimationFrame(this.animate);

      // cube.rotation.x += 0.01;
      // cube.rotation.y += 0.01;
      // controls.update();
      // console.log(cube.position);

      // renderer.render(scene, camera);
    },
  },
  mounted: function () {
    this.init();
    // this.animate();
  },
};
</script>

<style>
body {
  margin: 0;
}
</style>
