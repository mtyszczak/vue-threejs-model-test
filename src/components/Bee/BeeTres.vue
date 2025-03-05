<script lang="ts" setup>
import { useAnimations, useGLTF } from '@tresjs/cientos';
import { ref } from 'vue';
import bee from '../../models/animbee.glb';

const { scene: model, nodes, animations } = await useGLTF(bee);

for(const nodeIndex in nodes)
  nodes[nodeIndex]

const { actions } = useAnimations(animations, model);
const action2 = ref(actions['IcosphereAction.002']);
action2.value.play()
</script>

<template>
  <primitive :object="model" />
</template>
