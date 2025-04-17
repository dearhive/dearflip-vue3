<script setup>
import DearFlipViewer from '@/components/DearFlipViewer.vue';
import { onMounted, ref } from 'vue';

const flipbookOptions = {
  backgroundColor: '#ffffff',
  viewerType: 'flipbook',
  is3D: true,
  source: '/pdf/the-three-musketeers.pdf', // Update this path to your PDF source

  // Control settings
  autoEnableOutline: false,
  autoEnableThumbnail: false,
  overwritePDFOutline: false,

  // UI configuration
  webglShadow: true,
  transparent: false,
  hard: 'cover',            // 'cover', 'all', 'none'

  // Text settings
  text: {
    toggleSounds: "Sound",
    toggleThumbnails: "Thumbnails",
    toggleOutline: "Contents",
    previousPage: "Previous Page",
    nextPage: "Next Page",
    toggleFullscreen: "Fullscreen",
    zoomIn: "Zoom In",
    zoomOut: "Zoom Out",
    // ... more text options
  }
};

const dflipRef = ref(null);

onMounted(() => {
  const flipbook = dflipRef.value.getFlipbookInstance();
  // Example methods
  // flipbook.openFile(file);
})

const openFile = () => {
  dflipRef.value.getFlipbookInstance().openLocalFile();
}

</script>

<template>
  <button @click="openFile">Open File</button>
  <DearFlipViewer ref="dflipRef" :options="flipbookOptions" />

</template>
