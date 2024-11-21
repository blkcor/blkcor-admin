<template>
  <svg class="icon" aria-hidden="true">
    <use :xlink:href="className"></use>
  </svg>
</template>

<script setup lang="ts">
const props = defineProps({
  url: {
    type: String,
    default: ''
  },
  prefix: {
    type: String,
    default: '#icon-'
  },
  name: {
    type: String,
    default: ''
  }
})

const className = computed(() => `${props.prefix}${props.name}`)

onBeforeMount(() => {
  if (props.url) {
    const existingScript = document.querySelector(`script[src="${props.url}"]`)
    if (!existingScript) {
      const script = document.createElement('script')
      script.src = props.url
      document.head.appendChild(script)
    }
  }
})
</script>

<style scoped></style>
