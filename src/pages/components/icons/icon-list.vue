<template>
  <div class="flex gap-10 items-center">
    <el-switch
      v-model="copyTypeFlag"
      size="large"
      active-text="复制svg图标"
      inactive-text="复制icon名称"
    />

    <el-switch v-model="showText" size="large" active-text="显示文字" inactive-text="隐藏文字" />
    <el-checkbox label="复制icon组件" v-model="copyIconComponent" />
  </div>
  <IconListComp :show-text="showText" @click="handleCopyIcon" />
</template>

<script setup lang="ts">
import { loadIcon, type IconifyIcon } from '@iconify/vue'

const { copy, copied } = useClipboard()

// true => copy svg data
// false => copy icon name
const copyTypeFlag = ref(false)
const showText = ref(true)
const copyIconComponent = ref(false)

async function handleCopyIcon(icon: string) {
  if (copyIconComponent.value) {
    const copyText = `<i class="i-${icon}" />`
    copy(copyText)
    copied && ElMessage.success('复制成功')
  } else {
    if (copyTypeFlag.value) {
      const svg = await loadIcon(`${icon}`)
      const svgIcon = toSvgIcon(svg)
      copy(svgIcon)
      copied && ElMessage.success('复制成功')
    } else {
      copy(`ep-${icon}`)
      copied && ElMessage.success('复制成功')
    }
  }
}

function toSvgIcon(svg: Required<IconifyIcon>): string {
  // 处理变换属性
  let transform = ''
  const transforms: string[] = []
  if (svg.hFlip) transforms.push('scale(-1,1)')
  if (svg.vFlip) transforms.push('scale(1,-1)')
  if (svg.rotate) transforms.push(`rotate(${svg.rotate})`)
  if (transforms.length > 0) {
    transform = ` transform="${transforms.join(' ')}"`
  }

  // 构建 SVG 元素
  const svgElement = `<svg
      width="${svg.width || '1em'}"
      height="${svg.height || '1em'}"
      viewBox="0 0 ${svg.width || 1024} ${svg.height || 1024}"
      xmlns="http://www.w3.org/2000/svg"
      fill="currentColor"${transform}>
      ${svg.body}
    </svg>`
  return svgElement
}
</script>

<style scoped></style>
