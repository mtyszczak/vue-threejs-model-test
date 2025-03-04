<script lang="ts" setup>
import { useGLTF } from '@tresjs/cientos';
import cube from '../../models/cube.glb';

const { scene: model } = await useGLTF(cube);
</script>

<template>
  <primitive :object="model" />
</template>
