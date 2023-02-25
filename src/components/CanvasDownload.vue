<template>
  <Button @click="downloadCanvas">
    Download
  </Button>
</template>

<script setup>
import Button from './Button.vue'

const props = defineProps(['canvas'])

const downloadCanvas = () => {
  const link = document.createElement('a')

  link.download = 'wallpaper.png'
  link.href = props.canvas.toDataURL()
  link.click()
}
</script>
