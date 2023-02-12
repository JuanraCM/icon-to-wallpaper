<template>
  <h1>Captured resolution: {{ screenWidth }} x {{ screenHeight }}</h1>

  <div>
    <input type="file" accept="image/*" @change="onFileChange">
    <canvas ref="canvas" @click="downloadCanvas"></canvas>
  </div>
</template>

<script>
export default {
  data() {
    return {
      icon: new Image(),
      screenWidth: window.screen.width * window.devicePixelRatio,
      screenHeight: window.screen.height * window.devicePixelRatio
    }
  },
  computed: {
    canvas() {
      return this.$refs.canvas
    },

    ctx() {
      return this.canvas.getContext('2d')
    }
  },
  methods: {
    onFileChange(event) {
      const file = event.target.files[0]
      const imageUrl = URL.createObjectURL(file)

      this.icon.src = imageUrl
      this.icon.alt = file.name
      this.icon.onload = this.drawImage
    },

    drawImage() {
      const iconWidth = this.icon.naturalWidth
      const iconHeight = this.icon.naturalHeight

      this.canvas.width = this.screenWidth
      this.canvas.height = this.screenHeight

      const x = (this.canvas.width - iconWidth) / 2
      const y = (this.canvas.height - iconHeight) / 2

      const offscreenCanvas = document.createElement("canvas")
      offscreenCanvas.width = this.canvas.width
      offscreenCanvas.height = this.canvas.height
      const offscreenCtx = offscreenCanvas.getContext("2d")

      offscreenCtx.drawImage(this.icon, x, y)

      const borderData = offscreenCtx.getImageData(x, y, 1, 1)
      const [borderR, borderG, borderB, borderA] = borderData.data

      this.ctx.fillStyle = `rgba(${borderR}, ${borderG}, ${borderB}, ${borderA})`
      this.ctx.fillRect(0, 0, this.canvas.width, this.canvas.height)
      this.ctx.drawImage(offscreenCanvas, 0, 0)
    },

    downloadCanvas() {
      if (!this.icon.src) return

      const originalFileName = this.icon.alt.split('.')[0]
      const link = document.createElement('a')

      link.download = `${originalFileName}_${this.screenWidth}x${this.screenHeight}.png`
      link.href = this.canvas.toDataURL()
      link.click()
    }
  }
}
</script>
