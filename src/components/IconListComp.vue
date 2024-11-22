<template>
  <ul class="flex flex-wrap border rounded">
    <li
      :class="[
        'flex flex-col gap-2 border-r border-b items-center justify-center cursor-pointer',
        itemClass
      ]"
      v-for="icon in iconData"
      :key="icon"
      @click="handleCopyIcon(icon)"
    >
      <component :class="iconClass" :is="Icon" :icon="`${collection}-${icon}`" />
      <span v-if="showText" class="text-sm">{{ toPascalCase(icon) }}</span>
    </li>
  </ul>
</template>

<script setup lang="ts">
import { Icon, loadIcons } from '@iconify/vue'
import data from '@/components/icon-ep.json'

// true => copy svg data
// false => copy icon name

interface IconListType {
  iconData?: string[]
  collection?: string
  itemClass?: string
  iconClass?: string
  showText?: boolean
}
const emits = defineEmits(['click'])

const props = withDefaults(defineProps<IconListType>(), {
  iconData: () => data,
  collection: 'ep',
  itemClass: 'hover:bg-sky-100 w-1/8 py-4',
  iconClass: 'text-3xl',
  showText: true
})

async function handleCopyIcon(icon: string) {
  emits('click', icon)
}

function toPascalCase(str: string) {
  return str
    .split('-')
    .map((word) => word.charAt(0).toUpperCase() + word.slice(1))
    .join('')
}
onBeforeMount(() => {
  loadIcons(props.iconData)
})
</script>

<style scoped></style>
