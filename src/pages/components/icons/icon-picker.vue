<template>
  <icon-picker-comp @submit="handleSubmit" />
</template>

<script setup lang="ts">
const handleSubmit = (data: any) => {
  console.log('data:', data)
}
</script>

<style scoped></style>
