<script setup>
import { GUI } from "dat.gui";

import {
  Scene,
  PerspectiveCamera,
  WebGLRenderer,
  SphereGeometry,
  BoxGeometry,
  MeshBasicMaterial,
  MeshPhongMaterial,
  Mesh,
  Clock,
  TextureLoader,
  SpotLight,
  SpotLightHelper,
  AmbientLight,
  PCFSoftShadowMap,
  sRGBEncoding,
  CameraHelper,
} from "three";
import { FlyControls } from "three/examples/jsm/controls/FlyControls.js";

let scene,
  renderer,
  groundBox,
  sphere,
  camera,
  controls,
  clock,
  spotLight,
  spotLightHelper;

function init() {
  scene = new Scene();
  renderer = new WebGLRenderer();
  renderer.shadowMap.enabled = true;
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.shadowMap.enabled = true;
  renderer.shadowMap.type = PCFSoftShadowMap;
  renderer.outputEncoding = sRGBEncoding;
  document.querySelector("#app").appendChild(renderer.domElement);

  spotLight = new SpotLight(0xffffff);
  spotLight.position.set(0, 50, 0);
  spotLight.castShadow = true;
  spotLight.intensity = 1000;
  spotLight.distance = 200;
  spotLight.focus = 1;
  spotLight.shadow.mapSize.width = 512;
  spotLight.shadow.mapSize.height = 512;
  spotLight.shadow.camera.near = 10;
  spotLight.shadow.camera.far = 200;
  spotLight.shadow.focus = 1;

  const ambient = new AmbientLight(0xffffff, 0.1);
  scene.add(ambient);

  scene.add(spotLight);

  spotLightHelper = new SpotLightHelper(spotLight);
  scene.add(spotLightHelper);

  //set gui
  const gui = new GUI();
  gui.add(spotLight.position, "x", -100, 1000);
  gui.add(spotLight.position, "y", -100, 1000);
  gui.add(spotLight.position, "z", -100, 1000);
  gui.add(spotLight, "intensity", 0, 10);
  gui.add(spotLight, "distance", 0, 1000);
  gui.add(spotLight, "angle", 0, 2 * Math.PI);
  gui.add(spotLight, "penumbra", 0, 1);
  gui.add(spotLight, "decay", 0, 1);
  gui.add(spotLight, "focus", 0, 1.5);

  groundBox = new Mesh(
    new BoxGeometry(100, 1, 100),
    new MeshPhongMaterial({
      map: new TextureLoader().load("src/assets/floor.png"),
      color: 0xaaaaaa,
      specular: 0x333333,
      shininess: 25,
    })
  );
  groundBox = new Mesh(
    new BoxGeometry(100, 1, 100),
    new MeshBasicMaterial({
      map: new TextureLoader().load("src/assets/floor.png"),
    })
  );
  groundBox.position.set(0, -5, 0);
  scene.add(groundBox);

  sphere = new Mesh(
    new SphereGeometry(1, 16, 16),
    new MeshBasicMaterial({
      map: new TextureLoader().load("src/assets/Texture1.jpg"),
    })
  );
  sphere.position.set(0, 1, 0);
  sphere.receiveShadow = true;
  scene.add(sphere);

  camera = new PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );
  camera.position.z = 5;

  const shadowCameraHelper = new CameraHelper(spotLight.shadow.camera);
  scene.add(shadowCameraHelper);

  controls = new FlyControls(camera, renderer.domElement);
  controls.movementSpeed = 10;
  controls.rollSpeed = 1;
  controls.autoForward = false;
  controls.dragToLook = true;
  clock = new Clock();

  setCanvaSize();
}

function setCanvaSize() {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
}

window.addEventListener("resize", setCanvaSize, false);

const animate = () => {
  spotLightHelper.update();
  controls.update(clock.getDelta());
  requestAnimationFrame(animate);
  renderer.render(scene, camera);
};

init();
animate();
</script>

<template>
  <div></div>
</template>
