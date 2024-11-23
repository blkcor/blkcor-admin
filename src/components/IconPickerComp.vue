<template>
  <el-button type="primary" @click="() => toggle(true)">选择图标</el-button>
  <el-dialog :width="width" v-model="show">
    <template #header="{ titleId, titleClass }">
      <div class="flex justify-center">
        <h2 :id="titleId" class="fw-800">图标选择器 🥑</h2>
      </div>
    </template>
    <!-- iconList -->
    <icon-list-comp
      item-class="hover:bg-sky-100"
      icon-class="text-2xl"
      :show-text="false"
      @click="handleClick"
      active-class="text-[#409eff]"
    />
    <!-- setting -->
    <div class="flex items-center mt-4">
      <el-color-picker v-model="color" />
      <el-input-number v-model="size" :step="1" />
    </div>
    <div class="flex items-center mt-3">
      选中的图标：<Icon :icon="iconRef" :style="{ color: color, fontSize: `${size}px` }" />
    </div>
    <template #footer>
      <el-button @click="() => toggle(false)">取 消</el-button>
      <el-button type="primary" @click="handleConfirm">确 定</el-button>
    </template>
  </el-dialog>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue'
import type { IconPickerSubmitType } from './type'

const size = ref(16)
const color = ref('#409eff')
const iconRef = ref('')
const [show, toggle] = useToggle(false)

defineProps<{
  width: {
    type: String
    default: '50%'
  }
}>()
const emits = defineEmits<{
  submit: [data: IconPickerSubmitType]
  cancel: []
}>()
const handleClick = (icon: string) => {
  iconRef.value = icon
}

const handleConfirm = () => {
  toggle(false)
  emits('submit', {
    icon: iconRef.value,
    color: color.value,
    size: size.value
  })
}
</script>

<style scoped></style>
