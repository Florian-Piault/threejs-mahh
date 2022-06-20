<script setup>
import { GUI } from "dat.gui";
import {
  Scene,
  PerspectiveCamera,
  WebGLRenderer,
  Clock,
  Color,
  Vector3,
  PlaneGeometry,
  TextureLoader,
  AmbientLight,
  Mesh,
  MeshBasicMaterial,
  MeshPhongMaterial,
} from "three";
import { PointerLockControls } from "three/examples/jsm/controls/PointerLockControls.js";

let scene, renderer, camera, controls, clock, light;

let moveForward = false;
let moveBackward = false;
let moveLeft = false;
let moveRight = false;
let canJump = false;
let position;
const velocity = new Vector3();
const direction = new Vector3();

const setCanvaSize = () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
};

const setRenderer = () => {
  renderer = new WebGLRenderer({ antialias: true });
  renderer.setPixelRatio(window.devicePixelRatio);
  renderer.setSize(window.innerWidth, window.innerHeight);
  window.addEventListener("resize", setCanvaSize, false);
  document.querySelector("#app").appendChild(renderer.domElement);
};

const setCamera = () => {
  camera = new PerspectiveCamera(
    75,
    window.innerWidth / window.innerHeight,
    0.1,
    1000
  );
  camera.position.z = 5;
  camera.position.y = 5;
  scene.add(camera);

  const gui = new GUI();
  gui.add(camera.position, "x", -5, 100);
  gui.add(camera.position, "y", -5, 100);
  gui.add(camera.position, "z", -5, 100);
};

const move = () => {
  if (controls.isLocked) return;

  const delta = clock.getDelta();

  velocity.x -= velocity.x * 10.0 * delta;
  velocity.z -= velocity.z * 10.0 * delta;

  direction.z = Number(moveForward) - Number(moveBackward);
  direction.x = Number(moveRight) - Number(moveLeft);
  direction.normalize();

  if (moveForward || moveBackward) velocity.z -= direction.z * 400.0 * delta;
  if (moveLeft || moveRight) velocity.x -= direction.x * 400.0 * delta;

  controls.moveRight(-velocity.x * delta);
  controls.moveForward(-velocity.z * delta);

  controls.getObject().position.y += velocity.y * delta;
};

const onKeyDown = event => {
  switch (event.code) {
    case "ArrowUp":
    case "KeyW":
      moveForward = true;
      break;

    case "ArrowLeft":
    case "KeyA":
      moveLeft = true;
      break;

    case "ArrowDown":
    case "KeyS":
      moveBackward = true;
      break;

    case "ArrowRight":
    case "KeyD":
      moveRight = true;
      break;

    case "Space":
      if (canJump === true) velocity.y += 350;
      canJump = false;
      break;
  }
};

const onKeyUp = event => {
  switch (event.code) {
    case "ArrowUp":
    case "KeyW":
      moveForward = false;
      break;

    case "ArrowLeft":
    case "KeyA":
      moveLeft = false;
      break;

    case "ArrowDown":
    case "KeyS":
      moveBackward = false;
      break;

    case "ArrowRight":
    case "KeyD":
      moveRight = false;
      break;
  }
};

const setControls = () => {
  controls = new PointerLockControls(camera, renderer.domElement);
  controls.movementSpeed = 10;
  controls.rollSpeed = 1;
  controls.autoForward = false;
  clock = new Clock();
  document.addEventListener("keydown", onKeyDown);
  document.addEventListener("keyup", onKeyUp);
};

const setScene = () => {
  scene = new Scene();
  scene.background = new Color(0x87ceeb);
  light = new AmbientLight(0x404040);
  scene.add(light);
};

const setFloor = () => {
  let floorGeometry = new PlaneGeometry(2000, 2000, 100, 100);
  position = floorGeometry.attributes.position;
  const floor = new Mesh(
    floorGeometry.rotateX(-Math.PI / 2),
    new MeshPhongMaterial({
      map: new TextureLoader().load("src/assets/floor.png"),
    })
  );
  scene.add(floor);
};

const init = () => {
  setScene();
  setRenderer();
  setCamera();
  setControls();
  setCanvaSize();
  setFloor();
  animate();
};

const animate = () => {
  requestAnimationFrame(animate);
  move();
  renderer.render(scene, camera);
};

init();
</script>

<template>
  <button @click="controls.lock()">Lock</button>
</template>
