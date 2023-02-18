<template>
  <button @click="downloadCanvas">Download</button>
</template>

<script>
  export default {
    props: ['canvas'],
    methods: {
      downloadCanvas() {
        const link = document.createElement('a')

        link.download = 'wallpaper.png'
        link.href = this.canvas.toDataURL()
        link.click()
      }
    }
  }
</script>
