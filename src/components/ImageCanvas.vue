<template>
  <ImageLoad @image-loaded="setImage" />
  <CanvasDownload :canvas="canvas" />

  <canvas ref="canvas"></canvas>
</template>

<script>
import ImageLoad from './ImageLoad.vue'
import CanvasDownload from './CanvasDownload.vue'

export default {
  components: {
    ImageLoad,
    CanvasDownload
  },
  data() {
    return {
      image: null,
      canvas: null,
      ctx: null,
      screenWidth: window.screen.width * window.devicePixelRatio,
      screenHeight: window.screen.height * window.devicePixelRatio
    }
  },
  mounted() {
    this.canvas = this.$refs.canvas
    this.ctx = this.canvas.getContext('2d')
  },
  methods: {
    drawCanvas() {
      const imageWidth = this.image.naturalWidth
      const imageHeight = this.image.naturalHeight

      this.canvas.width = this.screenWidth
      this.canvas.height = this.screenHeight

      const x = (this.canvas.width - imageWidth) / 2
      const y = (this.canvas.height - imageHeight) / 2

      const offscreenCanvas = document.createElement("canvas")
      offscreenCanvas.width = this.canvas.width
      offscreenCanvas.height = this.canvas.height
      const offscreenCtx = offscreenCanvas.getContext("2d")

      offscreenCtx.drawImage(this.image, x, y)

      const borderData = offscreenCtx.getImageData(x, y, 1, 1)
      const [borderR, borderG, borderB, borderA] = borderData.data

      this.ctx.fillStyle = `rgba(${borderR}, ${borderG}, ${borderB}, ${borderA})`
      this.ctx.fillRect(0, 0, this.canvas.width, this.canvas.height)
      this.ctx.drawImage(offscreenCanvas, 0, 0)
    },

    setImage(imageFile) {
      const image = new Image()
      const imageUrl = URL.createObjectURL(imageFile)

      image.src = imageUrl
      image.alt = imageFile.name
      image.onload = this.drawCanvas

      this.image = image
    }
  }
}
</script>
