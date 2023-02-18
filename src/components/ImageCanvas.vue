<template>
  <ImageLoad @image-loaded="setImage" />
  <CanvasDownload :canvas="canvas" />

  <canvas ref="canvas"></canvas>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import ImageLoad from './ImageLoad.vue'
import CanvasDownload from './CanvasDownload.vue'

const screenWidth = window.screen.width * window.devicePixelRatio
const screenHeight = window.screen.height * window.devicePixelRatio
const canvas = ref(null)

let icon = null
let ctx = null

onMounted(() => {
  ctx = canvas.value.getContext('2d')
})

const drawImage = () => {
  const iconWidth = icon.naturalWidth
  const iconHeight = icon.naturalHeight

  canvas.value.width = screenWidth
  canvas.value.height = screenHeight

  const x = (canvas.value.width - iconWidth) / 2
  const y = (canvas.value.height - iconHeight) / 2

  const offscreenCanvas = document.createElement("canvas")
  const offscreenCtx = offscreenCanvas.getContext("2d")

  offscreenCanvas.width = canvas.value.width
  offscreenCanvas.height = canvas.value.height
  offscreenCtx.drawImage(icon, x, y)

  const borderData = offscreenCtx.getImageData(x, y, 1, 1)
  const [borderR, borderG, borderB, borderA] = borderData.data

  ctx.fillStyle = `rgba(${borderR}, ${borderG}, ${borderB}, ${borderA})`
  ctx.fillRect(0, 0, canvas.value.width, canvas.value.height)
  ctx.drawImage(offscreenCanvas, 0, 0)
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
