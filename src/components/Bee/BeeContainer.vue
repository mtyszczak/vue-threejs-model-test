<script lang="ts" setup>
import { TresCanvas } from '@tresjs/core';
import { OrbitControls } from '@tresjs/cientos';

import BeeTres from './BeeTres.vue';
</script>

<template>
  <TresCanvas alpha>
    <TresPerspectiveCamera />
    <OrbitControls />
    <TresAmbientLight :intensity="1" />
    <Suspense>
      <BeeTres />
    </Suspense>
  </TresCanvas>
</template>
