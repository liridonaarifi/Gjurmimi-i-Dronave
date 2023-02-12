<script setup lang="ts">
import MapTemplate from './components/MapTemplate.vue'
</script>

<template>


<div class="wrapper">
      <MapTemplate msg="Map is shown here!" />
    </div>
</template>



<style scoped>
header {
  line-height: 1.5;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }
}
</style>
