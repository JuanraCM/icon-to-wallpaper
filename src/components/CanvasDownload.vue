<template>
  <button @click="downloadCanvas">Download</button>
</template>

<script setup>
const props = defineProps(['canvas'])

const downloadCanvas = () => {
  const link = document.createElement('a')

  link.download = 'wallpaper.png'
  link.href = props.canvas.toDataURL()
  link.click()
}
</script>
