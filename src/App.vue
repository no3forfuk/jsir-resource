<template>
  <div id="app" ref="app"></div>
</template>
<script>
import * as THREE from "three";
import OrbitControls from "three-orbitcontrols";
export default {
  data() {
    return {};
  },
  mounted() {
    this.initScene();
  },
  methods: {
    initScene() {
      const scene = new THREE.Scene();
      const container = this.$refs.app;
      const camera = new THREE.PerspectiveCamera(
        100,
        window.innerWidth / window.innerHeight,
        0.1,
        1000
      );
      const renderer = new THREE.WebGLRenderer({
        antialias: true
        // alpha: true
      });
      renderer.setSize(window.innerWidth, window.innerHeight);
      container.appendChild(renderer.domElement);
      var geometry = new THREE.BoxGeometry(1, 1, 1);
      var texture = new THREE.TextureLoader().load(
        require("./assets/230242_1246976.jpg")
      );
      texture.repeat.set(0.4, 0.4);
      var material = new THREE.MeshStandardMaterial({
        map: texture
        // color: "#f60"
      });
      scene.background = new THREE.CubeTextureLoader().load([
        require("./assets/px.jpg"),
        require("./assets/nx.jpg"),
        require("./assets/py.jpg"),
        require("./assets/ny.jpg"),
        require("./assets/pz.jpg"),
        require("./assets/nz.jpg")
      ]);
      var cube = new THREE.Mesh(geometry, material);
      cube.castShadow = true;
      scene.add(cube);
      scene.add(this.ground());
      camera.position.z = 5;

      const controls = new OrbitControls(camera, renderer.domElement);
      controls.update();
      // var spotLight = new THREE.SpotLight("#fff");
      // spotLight.position.set(2, 5, -0.5);

      // spotLight.castShadow = true;
      // scene.add(spotLight);
      var light = new THREE.AmbientLight("#404040"); // soft white light
      scene.add(light);

      // var spotLightHelper = new THREE.SpotLightHelper(spotLight);
      // scene.add(spotLightHelper);
      renderer.shadowMap.enabled = true;
      renderer.render(scene, camera);
      function animate() {
        requestAnimationFrame(animate);

        // required if controls.enableDamping or controls.autoRotate are set to true
        controls.update();
        renderer.render(scene, camera);
      }
      animate();
    },
    ground() {
      var geometry = new THREE.BoxGeometry(200, 0.01, 200);

      var texture = new THREE.TextureLoader().load(
        "./assets/211913_1262110.jpg"
      );
      var material = new THREE.MeshStandardMaterial({
        // map: texture
        color: "#fff"
      });
      var cube = new THREE.Mesh(geometry, material);
      cube.receiveShadow = true;
      cube.position.y = -20;
      return cube;
    }
  }
};
</script>
<style lang="less">
</style>
