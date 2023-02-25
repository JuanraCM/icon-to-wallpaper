<template>
  <div class="container m-auto">
    <AppHeader />

    <main>
      <ImageCanvas />
    </main>

    <AppFooter />
  </div>
</template>

<script setup>
import AppHeader from './components/AppHeader.vue'
import ImageCanvas from './components/ImageCanvas.vue'
import AppFooter from './components/AppFooter.vue'
</script>
