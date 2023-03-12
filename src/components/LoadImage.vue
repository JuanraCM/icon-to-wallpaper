<template>
  <Button @click="loadImage">
    Load image
  </Button>
</template>

<script setup>
import Button from './Button.vue'

const emit = defineEmits(['image-loaded'])

const loadImage = () => {
  const input = document.createElement('input')

  input.type = 'file'
  input.accept = 'image/*'
  input.onchange = _onFileChange

  input.click()
}

const _onFileChange = (event) => {
  const file = event.target.files[0]
  if (file) emit('image-loaded', file)
}
</script>
