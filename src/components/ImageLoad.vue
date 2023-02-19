<template>
  <input type="file" accept="image/*" @change="onFileChange" />
</template>

<script setup>
const emit = defineEmits(['image-loaded'])

const onFileChange = (event) => {
  const file = event.target.files[0]
  if (file) emit('image-loaded', file)
}
</script>
