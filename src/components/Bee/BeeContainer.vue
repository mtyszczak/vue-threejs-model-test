<script lang="ts" setup>
import * as THREE from 'three';
import { GLTFLoader } from 'three/addons/loaders/GLTFLoader.js';
import { onMounted, ref } from 'vue';
import beeModel from '../../models/animbee.glb';

const container = ref<HTMLElement | null>(null);

onMounted(() => {
  const scene = new THREE.Scene();
  const camera = new THREE.PerspectiveCamera( 75, window.innerWidth / window.innerHeight, 0.1, 1000 );

  const renderer = new THREE.WebGLRenderer({ alpha: true });
  renderer.toneMapping = THREE.LinearToneMapping; // Other options: No, Linear, etc.
  renderer.toneMappingExposure = 1.5; // Adjust the exposure intensity
  renderer.setSize( window.innerWidth, window.innerHeight );
  container.value!.appendChild( renderer.domElement );

  const ambientLight = new THREE.AmbientLight(0xffffff, 1); // Increase intensity
  scene.add(ambientLight);

  const directionalLight = new THREE.DirectionalLight(0xffffff, 1.5); // Increase intensity
  directionalLight.position.set(1, 1, 1).normalize();
  scene.add(directionalLight);

  // Add another light for better illumination if needed
  const pointLight = new THREE.PointLight(0xffffff, 0.5);
  pointLight.position.set(0, 2, 2);
  scene.add(pointLight);

    // Instantiate a loader
  const loader = new GLTFLoader();

  let model: THREE.Group<THREE.Object3DEventMap>, mixer: THREE.AnimationMixer, clock: THREE.Clock;

  const animate = () => {
    const delta = clock.getDelta();
    const elapsed = clock.getElapsedTime();
    mixer.update(delta);
    model.position.x = Math.sin(elapsed) * 3;
    model.position.z = Math.cos(elapsed) * 1.5;
    model.position.y = Math.sin(elapsed * 2);
    model.rotation.z = -(Math.sin(elapsed) / 1.5);
    renderer.render(scene, camera);
  }
  // Load a glTF resource
  loader.load(
    beeModel,
    gltf => {
      gltf.scene.traverse((node: any) => {
        if (node.isMesh) {
          node.material.emissiveIntensity = 1; // Keep or adjust as needed
          node.material.roughness = 0.5; // Lower roughness for more shine
          node.material.metalness = 0; // Ensure metalness is low if not needed
        }
      });

      console.log(gltf);

      scene.add( model = gltf.scene );

      clock = new THREE.Clock();

      mixer = new THREE.AnimationMixer( gltf.scene );
      mixer.clipAction( gltf.animations[1] ).loop = THREE.LoopRepeat;
      mixer.clipAction( gltf.animations[1] ).play();

      model.rotation.y = Math.PI / 2; // Rotate the model if needed
      renderer.setAnimationLoop( animate );
    }
  );

  camera.position.z = 5;
  window.addEventListener('resize', () => {
    camera.aspect = window.innerWidth / window.innerHeight;
    camera.updateProjectionMatrix();
    renderer.setSize( window.innerWidth, window.innerHeight );
  });
});
</script>

<template>
  <div ref="container" style="width: 100%; height: 100%;"></div>
</template>
