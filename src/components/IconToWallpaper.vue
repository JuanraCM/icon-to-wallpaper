<template>
  <Preview :screen="screen" :wallpaper="wallpaper" />

  <div class="flex justify-center">
    <LoadImage @image-loaded="setImage" />
    <DownloadWallpaper :wallpaper="wallpaper" />
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Preview from './Preview.vue'
import LoadImage from './LoadImage.vue'
import DownloadWallpaper from './DownloadWallpaper.vue'

const screen = {
  width: window.screen.width * window.devicePixelRatio,
  height: window.screen.height * window.devicePixelRatio
}

const wallpaper = ref('')

const setImage = (imageFile) => {
  const image = new Image()
  const imageUrl = URL.createObjectURL(imageFile)

  image.src = imageUrl
  image.alt = imageFile.name
  image.onload = _drawImage
}

const _drawImage = ({ target: image }) => {
  const [wpCanvas, wpCtx] = _buildCanvas()
  const [auxCanvas, auxCtx] = _buildCanvas()

  const x = (wpCanvas.width - image.naturalWidth) / 2
  const y = (wpCanvas.height - image.naturalHeight) / 2

  auxCtx.drawImage(image, x, y)

  const borderData = auxCtx.getImageData(x, y, 1, 1)
  const [borderR, borderG, borderB, borderA] = borderData.data

  wpCtx.fillStyle = `rgba(${borderR}, ${borderG}, ${borderB}, ${borderA})`
  wpCtx.fillRect(0, 0, wpCanvas.width, wpCanvas.height)
  wpCtx.drawImage(auxCanvas, 0, 0)

  wallpaper.value = wpCanvas.toDataURL()
}

const _buildCanvas = () => {
  const canvas = document.createElement('canvas')
  const ctx = canvas.getContext('2d')

  canvas.width = screen.width
  canvas.height = screen.height

  return [canvas, ctx]
}
</script>
