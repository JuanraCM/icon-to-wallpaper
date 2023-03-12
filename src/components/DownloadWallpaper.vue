<template>
  <Button @click="downloadCanvas">
    Download
  </Button>
</template>

<script setup>
import Button from './Button.vue'

const props = defineProps(['wallpaper'])

const downloadCanvas = () => {
  if (!props.wallpaper) return

  const link = document.createElement('a')

  link.download = 'wallpaper.png'
  link.href = props.wallpaper
  link.click()
}
</script>
