<template>
  <div>
    <div ref="preview" id="preview"></div>
  </div>
</template>

<script setup>
import { ref, toRef, watch } from 'vue'

const props = defineProps(['screen', 'wallpaper'])
const wallpaper = toRef(props, 'wallpaper')

const preview = ref(null)

watch(wallpaper, (newValue, _oldValue) => {
  if (newValue) _setPreviewWallpaper(newValue)
})

const _setPreviewWallpaper = (imageData) => {
  preview.value.style.backgroundImage = `url('${imageData}')` 
}
</script>

<style scoped>
#preview {
  @apply max-w-full m-10 aspect-auto border-2 flex items-center justify-center bg-cover;
  aspect-ratio: v-bind('props.screen.width') / v-bind('props.screen.height');
}

#preview::before {
  @apply -z-10 text-4xl text-gray-500 font-semibold;
  content: 'Preview';
}
</style>