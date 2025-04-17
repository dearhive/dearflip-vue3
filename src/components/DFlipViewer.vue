<template>
  <div data-option="dflipOptions" class="df-element"></div>
</template>

<script setup>
import { onMounted, onBeforeUnmount, defineProps, defineExpose } from 'vue'

const props = defineProps({
  options: {
    type: Object,
    default: () => ({})
  }
})

let flipbookInstance = null;

onMounted(() => {
  // Set correct asset locations
  window.dFlipLocation = "/dflip/";

  // Initialize the flipbook with the provided options
  window.dflipOptions = props.options;

  // Initialize the flipbook
  try {
    flipbookInstance = window.DEARFLIP;
    console.log("Flipbook initialized successfully:", flipbookInstance);
  } catch (error) {
    console.error("Error initializing flipbook:", error);
  }
})

onBeforeUnmount(() => {
  // Cleanup when component is destroyed
  if (flipbookInstance && typeof flipbookInstance.dispose === 'function') {
    flipbookInstance.dispose()
    flipbookInstance = null;
    console.log("disposed");
  }
})


defineExpose({
  getFlipbookInstance: () => flipbookInstance
})
</script>



<style scoped>
.df-element {
  width: 100%;
  height: 100%;
  position: relative;
}
</style>
