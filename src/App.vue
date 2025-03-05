<script setup lang="ts">
import Bee from './components/Bee';
</script>

<template>
  <main style="background-color: gray">
    <div style="width: 100%; height: 100lvh;">
      <div style="position: relative; width: 100%; height: 100%; overflow: hidden; pointer-events: none;">
        <Bee />
      </div>
    </div>
  </main>
</template>