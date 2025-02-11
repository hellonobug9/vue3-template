<script setup lang="ts">
const url =
  'https://devgrapeseedmaterial.blob.core.windows.net/gl-content-audio/GSv4-U9-REP-Everything%20Green.mp3?sv=2018-03-28&sr=b&sig=yzioQvT5lASZbK4s4rtVD3rQb9oihL2MdpkwpzKhoQo%3D&st=2025-01-02T09%3A18%3A15Z&se=2025-01-02T11%3A18%3A15Z&sp=r'
</script>

<template>
  <div class="relative py-8 flex justify-center items-center h-screen">
    <audio controls>
      <source :src="url" type="audio/mpeg" />
      </audio>
  </div>
</template>

<style>
/* Add any additional styles here */
</style>
