<template>
  <input type="file" accept="image/*" @change="onFileChange" />
</template>

<script>
  export default {
    emits: ['image-loaded'],
    methods: {
      onFileChange(event) {
        const file = event.target.files[0]
        if (file) this.$emit('image-loaded', file)
      }
    }
  }
</script>
