<template>
  <div class="icon-picker">
    <el-button type="primary" @click="() => toggle(true)">选择图标</el-button>
    <el-dialog :width="width" v-model="show">
      <template #header="{ titleId }">
        <div class="flex justify-center">
          <h2 :id="titleId" class="fw-800" v-text="`${props.title}  🥑`"></h2>
        </div>
      </template>
      <!-- iconList -->
      <icon-list-comp
        item-class="hover:bg-sky-100"
        icon-class="text-2xl"
        :show-text="false"
        @click="handleClick"
        active-class="text-[#409eff]"
        v-model="iconRef"
      />
      <!-- setting -->
      <div class="flex flex-col gap-2 mt-4">
        <div class="flex items-center gap-2">
          <span>🍉 选择颜色: </span>
          <el-color-picker v-model="color" />
        </div>
        <div class="flex items-center gap-2 pr-4">
          <span>🍓 选择大小</span>
          <el-slider v-model="size" :min="12" :max="48" :step="1" show-input class="flex-1" />
        </div>
      </div>
      <div class="flex items-center mt-3">
        选中的图标：<Icon :icon="iconRef" :style="{ color: color, fontSize: `${size}px` }" />
      </div>
      <template #footer>
        <el-button @click="() => toggle(false)">取 消</el-button>
        <el-button type="primary" @click="handleConfirm">确 定</el-button>
      </template>
    </el-dialog>
  </div>
</template>

<script setup lang="ts">
import { Icon } from '@iconify/vue'
import type { IconPickerSubmitType } from './type'

const size = ref(16)
const color = ref('#409eff')
const iconRef = ref('')
const [show, toggle] = useToggle(false)

const props = defineProps({
  width: {
    type: String,
    default: '50%'
  },
  title: {
    type: String,
    default: '图标选择器'
  }
})

console.log(props)
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

<style scoped>
.icon-picker :deep(.el-dialog__body) {
  height: auto;
  max-height: 65vh;
  padding-top: 0;
  padding-bottom: 0;
  overflow-y: auto;
}
</style>
