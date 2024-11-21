<template>
  <div>
    <component :is="Icon" :icon="iconRef" class="text-sky text-3xl"></component>
    <svg-icon name="activity" class="text-red w-10 h-10" />
    <network-icon
      class="text-3xl"
      name="a-091_anquan"
      url="//at.alicdn.com/t/c/font_4644311_n4nvf5ewys8.js"
    />
  </div>
</template>

<script setup lang="ts">
import { registerSW } from 'virtual:pwa-register'
import { Icon, addAPIProvider } from '@iconify/vue'
import academicons from '@iconify/json/json/academicons.json'

console.log(academicons)
const icons = Object.keys(academicons.icons)
const iconRef = ref('')

addAPIProvider('local', {
  resources: ['http://localhost:5173']
})

onMounted(async () => {
  setInterval(() => {
    iconRef.value = '@local:' + 'academicons:' + icons[Math.floor(Math.random() * icons.length)]
  }, 1000)
  registerSW({
    immediate: true,
    onRegisteredSW(_url, registration) {
      console.log('🚀 ~ onRegisteredSW ~ registration:', registration)
      console.log('🚀 ~ onRegisteredSW ~ _url:', _url)
      setInterval(() => {
        registration && registration.update()
      }, 3600000)
    }
  })
})
</script>

<style scoped></style>

<route lang="yaml">
meta:
  layout: default
</route>
