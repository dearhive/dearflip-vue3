# Integrating DearFlip PDF Viewer with Vue 3

This repository demonstrates how to integrate the DearFlip PDF viewer library with a Vue 3 application.

## Prerequisites
- Vue 3 project (created with Vite or Vue CLI)
- DearFlip library files (purchased from [DearFlip](https://dearflip.com/))

## Step-by-Step Integration Guide

### 1. Set up the directory structure

Place the DearFlip library files in the `public` folder of your Vue project:

```
public/
  └── dFlip/
      ├── css/
      │   ├── dFlip.css
      │   └── dFlip.min.css
      ├── js/
      │   ├── dFlip.js
      │   ├── dFlip.min.js
      │   └── libs/
      ├── images/
      └── sound/
```

### 2. Add DearFlip resources to index.html

Modify your `index.html` to include the necessary dFlip CSS and JavaScript files:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.ico" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vue dFlip Integration</title>
    <!-- Include dFlip CSS -->
    <link rel="stylesheet" href="/dFlip/css/dFlip.css">
  </head>
  <body>
    <div id="app"></div>
    <!-- Include jQuery if required by your dFlip version -->
    <script src="/dFlip/js/libs/jquery.min.js"></script>
    <!-- Include dFlip JS -->
    <script src="/dFlip/js/dFlip.js"></script>
    <script type="module" src="/src/main.js"></script>
  </body>
</html>
```

### 3. Create a DearFlipViewer component

Create a `DearFlipViewer.vue` component in your components directory:

```javascript
// src/components/DearFlipViewer.vue
<template>
  <div data-option="dearFlipOptions" class="df-element"></div>
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
  window.dearFlipOptions = props.options;

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
```

### 4. Use the DearFlipViewer component in your app

In your main App.vue or wherever you want to display the PDF:

```javascript
<script setup>
import DearFlipViewer from '@/components/DearFlipViewer.vue';
import { onMounted, ref } from 'vue';

const flipbookOptions = {
  backgroundColor: '#ffffff',
  viewerType: 'flipbook',
  is3D: true,
  source: '/pdf/your-pdf-file.pdf', // Path to your PDF file
};

const dearFlip = ref(null);

onMounted(() => {
  // Access the flipbook instance if needed
  console.log(dearFlip.value.getFlipbookInstance());
})
</script>

<template>
  <DearFlipViewer ref="dearFlip" :options="flipbookOptions" />
</template>
```

### 5. Configure DearFlip options

The DearFlip viewer is highly customizable. Here are some common options you can include: For more info, visit [https://dearflip.com/docs](https://dearflip.com/docs)

```javascript
const flipbookOptions = {
  // Source PDF file
  source: '/pdf/your-pdf-file.pdf',
  
  // Appearance 
  backgroundColor: '#ffffff',
  viewerType: 'flipbook', // or 'slider', 'reader', etc.
  is3D: true,              // false for 2D flip
  
  // Control settings
  autoEnableOutline: false,
  autoEnableThumbnail: false,
  overwritePDFOutline: false,
  
  // UI configuration
  webglShadow: true,
  transparent: false,
  hard: 'none',            // 'cover', 'all', 'none'
  
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
```

### 6. Additional styling (optional)

You can add additional styling for the DearFlip viewer container in your App.vue:

```css
<style>
#app {
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

/* Make the viewer full width or specific dimensions */
.df-element {
  width: 80%;
  height: 80vh;
}
</style>
```

## Troubleshooting

1. **PDF not loading:** Ensure the path to your PDF file is correct and the file exists in the public folder.
2. **DearFlip not initializing:** Check browser console for errors and ensure all required dFlip files are correctly included.
3. **Styling issues:** Make sure the container div has appropriate dimensions (height/width).

## Advanced Usage

### Accessing dFlip API methods

You can access the dFlip instance to call its API methods:

```javascript
onMounted(() => {
  // Get the flipbook instance
  const flipbook = dearFlip.value.getFlipbookInstance();
  
  // Example methods
  // flipbook.openLocalFile();   // Open local pdf file in lightbox
});
```

### Event Handling

Add event listeners in the mounted hook:

```javascript
onMounted(() => {
    flipbook.defaults.onReady = (app) => {
        console.log('dFlip initialized');
    }
});
```
