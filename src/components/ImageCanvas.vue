<template>
  <div>
    <div ref="preview" id="preview"></div>

    <div class="flex justify-center">
      <ImageLoad @image-loaded="setImage" />
      <CanvasDownload :canvas="canvas" />
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import ImageLoad from './ImageLoad.vue'
import CanvasDownload from './CanvasDownload.vue'

const screenWidth = window.screen.width * window.devicePixelRatio
const screenHeight = window.screen.height * window.devicePixelRatio

const canvas = document.createElement("canvas")
const ctx = canvas.getContext('2d')

const preview = ref(null)

let icon = null

const drawImage = () => {
  const iconWidth = icon.naturalWidth
  const iconHeight = icon.naturalHeight

  canvas.width = screenWidth
  canvas.height = screenHeight

  const x = (canvas.width - iconWidth) / 2
  const y = (canvas.height - iconHeight) / 2

  const offscreenCanvas = document.createElement("canvas")
  const offscreenCtx = offscreenCanvas.getContext("2d")

  offscreenCanvas.width = canvas.width
  offscreenCanvas.height = canvas.height
  offscreenCtx.drawImage(icon, x, y)

  const borderData = offscreenCtx.getImageData(x, y, 1, 1)
  const [borderR, borderG, borderB, borderA] = borderData.data

  ctx.fillStyle = `rgba(${borderR}, ${borderG}, ${borderB}, ${borderA})`
  ctx.fillRect(0, 0, canvas.width, canvas.height)
  ctx.drawImage(offscreenCanvas, 0, 0)

  preview.value.style.backgroundImage = `url('${canvas.toDataURL()}')`
}

const setImage = (imageFile) => {
  const image = new Image()
  const imageUrl = URL.createObjectURL(imageFile)

  image.src = imageUrl
  image.alt = imageFile.name
  image.onload = drawImage

  icon = image
}
</script>

<style scoped>
#preview {
  @apply max-w-full m-10 aspect-auto border-2 flex items-center justify-center bg-cover;
  aspect-ratio: v-bind('screenWidth') / v-bind('screenHeight');
}

#preview::before {
  @apply -z-10 text-4xl text-gray-500 font-semibold;
  content: 'Preview';
}
</style>
