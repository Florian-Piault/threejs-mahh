<script setup>
import Icon from "./Icon.vue";
import * as THREE from "three";
import { GUI } from "dat.gui";
import * as TWEEN from "@tweenjs/tween.js";
import { ref } from "vue";

let scene, camera, renderer;
let actualObjectIndex = ref(0);
let isZoomedIn = ref(false);
let isZoomedOut = ref(false);

scene = new THREE.Scene();
camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);
camera.position.x = -45;
camera.position.y = 5;
camera.position.z = 0;
camera.rotateX = 0;
camera.rotateY = 0;
let objects = [
  { x: camera.position.x, y: camera.position.y, z: camera.position.z },
];

scene.add(camera);
scene.background = new THREE.Color(0x87ceeb);

renderer = new THREE.WebGLRenderer();
renderer.setPixelRatio(window.devicePixelRatio);
renderer.setSize(window.innerWidth, window.innerHeight);
document.querySelector("#app").appendChild(renderer.domElement);

const geometryPlane = new THREE.PlaneGeometry(1000, 1000, 10, 10);
geometryPlane.rotateX(-Math.PI / 2);
const texturePlane = new THREE.TextureLoader().load("src/assets/plane.jpg");
texturePlane.wrapS = texturePlane.wrapT = THREE.RepeatWrapping;
texturePlane.repeat.set(50, 50);
const materialPlane = new THREE.MeshBasicMaterial({ map: texturePlane });
const plane = new THREE.Mesh(geometryPlane, materialPlane);
scene.add(plane);

//#region gui
// const gui = new GUI();
// const guiPosition = gui.addFolder("Camera position");
// const guiRotation = gui.addFolder("Camera rotation");
// guiPosition.add(camera.position, "x", -100, 100);
// guiPosition.add(camera.position, "z", -100, 100);
// guiRotation.add(camera.rotation, "x", -Math.PI * 2, Math.PI * 2);
// guiRotation.add(camera.rotation, "y", -Math.PI * 2, Math.PI * 2);
// guiRotation.add(camera.rotation, "z", -Math.PI * 2, Math.PI * 2);
//#endregion

function setWalls() {
  const x = -50,
    y = 1.5,
    z = -20;

  const geometryWall = new THREE.BoxGeometry(5, 3, 10);
  const textureWall = new THREE.TextureLoader().load("src/assets/wall.jpeg");
  const materialWall = new THREE.MeshBasicMaterial({ map: textureWall });

  for (let i = 0; i < 21; i++) {
    const posX = x + 5 * i;
    const wall = new THREE.Mesh(geometryWall, materialWall);
    const wall2 = new THREE.Mesh(
      geometryWall,
      i % 2
        ? // ? new THREE.MeshBasicMaterial({ color: Math.random() * 0xffffff })
          new THREE.MeshBasicMaterial({
            map: new THREE.TextureLoader().load("src/assets/jocondedab.png"),
          })
        : materialWall
    );
    if (i % 2) objects.push({ x: posX, y: y + 3, z });
    const wall3 = new THREE.Mesh(geometryWall, materialWall);

    wall.position.set(posX, y, z);
    wall2.position.set(posX, y + 3, z);
    wall3.position.set(posX, y + 6, z);
    scene.add(wall);
    scene.add(wall2);
    scene.add(wall3);
  }

  const wallPlane = new THREE.PlaneGeometry(100, 15, 10, 10);
  wallPlane.rotateY(-Math.PI / 2);
  const textureWallPlane = new THREE.TextureLoader().load(
    "src/assets/wall.jpeg"
  );
  textureWallPlane.wrapS = textureWallPlane.wrapT = THREE.RepeatWrapping;
  textureWallPlane.repeat.set(20, 20);
  const materialWallPlane = new THREE.MeshBasicMaterial({
    map: textureWallPlane,
    side: THREE.DoubleSide,
  });
  const wallPlaneMesh = new THREE.Mesh(wallPlane, materialWallPlane);
  const wallPlaneMesh2 = new THREE.Mesh(wallPlane, materialWallPlane);
  wallPlaneMesh.position.set(-50, 1.5, 30);
  wallPlaneMesh2.position.set(50, 1.5, 30);
  scene.add(wallPlaneMesh);
  scene.add(wallPlaneMesh2);
}

const goToPrevious = () => {
  isZoomedIn.value = false;
  isZoomedOut.value = false;
  const { x, y, z } = objects[--actualObjectIndex.value];
  const coords = {
    x: camera.position.x,
    y: camera.position.y,
    z: camera.position.z,
  };
  new TWEEN.Tween(coords)
    .to({ x, y, z: z + 8 }, 1000)
    .easing(TWEEN.Easing.Quadratic.InOut)
    .onUpdate(() => {
      camera.position.set(coords.x, coords.y, coords.z);
    })
    .start();
};
const goToNext = () => {
  isZoomedIn.value = false;
  isZoomedOut.value = false;
  const { x, y, z } = objects[++actualObjectIndex.value];
  const coords = {
    x: camera.position.x,
    y: camera.position.y,
    z: camera.position.z,
  };
  new TWEEN.Tween(coords)
    .to({ x, y, z: z + 8 }, 1000)
    .easing(TWEEN.Easing.Quadratic.InOut)
    .onUpdate(() => {
      camera.position.set(coords.x, coords.y, coords.z);
    })
    .start();
};
const reset = () => {
  isZoomedIn.value = false;
  isZoomedOut.value = false;
  actualObjectIndex.value = 0;
  const { x, y, z } = objects[actualObjectIndex.value];
  const coords = {
    x: camera.position.x,
    y: camera.position.y,
    z: camera.position.z,
  };
  new TWEEN.Tween(coords)
    .to({ x, y, z }, 1000)
    .easing(TWEEN.Easing.Quadratic.InOut)
    .yoyo(true)
    .onUpdate(() => {
      camera.position.set(coords.x, coords.y, coords.z);
    })
    .start();
};

const zoomOut = () => {
  isZoomedOut.value = true;
  isZoomedIn.value = false;
  const { x, y, z } = objects[actualObjectIndex.value];
  const coords = {
    x: camera.position.x,
    y: camera.position.y,
    z: camera.position.z,
  };
  new TWEEN.Tween(coords)
    .to({ x, y, z: z + 8 }, 1000)
    .easing(TWEEN.Easing.Quadratic.InOut)
    .yoyo(true)
    .onUpdate(() => {
      camera.position.set(coords.x, coords.y, coords.z);
    })
    .start();
};
const zoomIn = () => {
  isZoomedIn.value = true;
  isZoomedOut.value = false;
  const { x, y, z } = objects[actualObjectIndex.value];
  const coords = {
    x: camera.position.x,
    y: camera.position.y,
    z: camera.position.z,
  };
  new TWEEN.Tween(coords)
    .to({ x, y: 4.5, z: -13 }, 1000)
    .easing(TWEEN.Easing.Quadratic.InOut)
    .yoyo(true)
    .onUpdate(() => {
      camera.position.set(coords.x, coords.y, coords.z);
    })
    .start();
};

const setCanvaSize = () => {
  camera.aspect = window.innerWidth / window.innerHeight;
  camera.updateProjectionMatrix();
  renderer.setSize(window.innerWidth, window.innerHeight);
};
window.addEventListener("resize", setCanvaSize, false);
setWalls();

function animate(callback) {
  function loop(time) {
    callback(time);
    requestAnimationFrame(loop);
  }
  requestAnimationFrame(loop);
}

animate(time => {
  renderer.render(scene, camera);
  TWEEN.update(time);
});
</script>

<template>
  <div class="quick-actions">
    <button
      @click="reset"
      :disabled="actualObjectIndex === 0"
      aria-lavel="réinitialiser"
      data-content="reset"
    >
      <Icon icon="reset" />
    </button>
    <button
      @click="zoomIn"
      :disabled="isZoomedIn"
      aria-lavel="zoomer"
      data-content="zoomIn"
    >
      <Icon icon="zoomIn" />
    </button>
    <button
      @click="zoomOut"
      :disabled="isZoomedOut"
      aria-lavel="dézoomer"
      data-content="zoomOut"
    >
      <Icon icon="zoomOut" />
    </button>
  </div>
  <button
    @click="goToPrevious"
    :disabled="actualObjectIndex === 0"
    aria-label="precedent"
    data-content="precedent"
  >
    <Icon icon="previous" />
  </button>
  <button
    @click="goToNext"
    :disabled="actualObjectIndex === objects.length - 1"
    aria-label="suivant"
    data-content="suivant"
  >
    <Icon icon="next" />
  </button>
</template>

<style scope>
button {
  position: absolute;
  z-index: 1;
  background: #fff;
  width: 50px;
  height: 50vh;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  border: 0;
  background-color: #ffffffa5;
}
button:disabled {
  cursor: auto;
}
button:is(:hover, :focus, :focus-visible):not(:disabled) {
  background-color: #dddddda5;
}
[data-content="precedent"] {
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  border-radius: 0 10px 10px 0;
}

[data-content="suivant"] {
  right: 0;
  top: 50%;
  transform: translateY(-50%);
  border-radius: 10px 0 0 10px;
}

.quick-actions {
  position: absolute;
  top: 0;
  right: 0;
  z-index: 1;
  display: flex;
  justify-content: flex-start;
  flex-direction: column;
  align-items: center;
  width: 50px;
  height: 50vh;
}
[data-content="zoomIn"],
[data-content="zoomOut"],
[data-content="reset"] {
  height: 50px;
  position: relative;
  transform: translateY(0);
  border-radius: 100vmax;
  padding: 1rem;
}
</style>
