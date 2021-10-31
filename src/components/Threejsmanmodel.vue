<template>
  <h3>Animating Cesium Man with ThreeJS</h3>
</template>

<script lang="ts">
import { defineComponent } from "@vue/runtime-core";
import * as THREE from "three";
import { TrackballControls } from "three/examples/jsm/controls/TrackballControls";
import { GLTFLoader } from "three/examples/jsm/loaders/GLTFLoader";
import { DRACOLoader } from "three/examples/jsm/loaders/DRACOLoader";
import { DirectionalLight } from "three";

export default defineComponent({
  name: "Threejsmanmodel",
});
// Scene
const scene = new THREE.Scene();
// Clock for counting animation
const clock = new THREE.Clock();
// GLTF loader for loading cesium man model gltf file
const loader = new GLTFLoader();
let mixer: any = null;
// Camera
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.6,
  1200
);
camera.position.y = 3; // Set camera position for y axis
camera.position.z = 4; // Set camera position z axis
// Renderer
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setClearColor("#000000"); // Set background colour to black
renderer.setSize(window.innerWidth, window.innerHeight);
document.body.appendChild(renderer.domElement); // Add renderer to HTML as a canvas element
const dracoloader = new DRACOLoader();
dracoloader.setDecoderPath("three/examples/js/libs/draco/");
loader.setDRACOLoader(dracoloader);

loader.load(
  "../gltf/sample-models/2.0/CesiumMan/glTF/CesiumMan.gltf",
  function (gltf) {
    // Logs for verifying gltf file loading
    console.log("Gltf");
    console.log(gltf);
    const root = gltf.scene;
    scene.add(root);
    // AnimationMixer for playing the animation with gltf file
    mixer = new THREE.AnimationMixer(gltf.scene);
    // Animation loop
    gltf.animations.forEach((clip) => {
      mixer.clipAction(clip).play();
    });
    root.scale.set(2, 2, 2);
  },
  function (xhr) {
    console.log((xhr.loaded / xhr.total) * 100) + "% loaded";
  },
  function (error) {
    // Log error
    console.log("An error occurred");
    console.log(error);
  }
);
// Set the light
const light = new DirectionalLight(0xffffff, 1);
light.position.set(2, 2, 5);
scene.add(light);
//Trackball Controls for Camera
const controls = new TrackballControls(camera, renderer.domElement);
controls.rotateSpeed = 4;
controls.dynamicDampingFactor = 0.15;

const rendering = function () {
  requestAnimationFrame(rendering);
  controls.update();
  // Constantly rotate box
  scene.rotation.z -= 0.005;
  scene.rotation.x -= 0.01;
  let delta = clock.getDelta();

  if (mixer) mixer.update(delta);
  renderer.render(scene, camera);
};

rendering();
</script>

<style></style>
