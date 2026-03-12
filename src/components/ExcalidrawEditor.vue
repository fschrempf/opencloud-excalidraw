<script setup lang="ts">
import { applyReactInVue } from 'veaury'
import { createRoot } from 'react-dom/client'
import ExcalidrawWrapper from '../react_app/ExcalidrawWrapper'

defineProps<{
  initialData?: string
  readOnly?: boolean
}>()

const emit = defineEmits<{
  change: [data: string]
  save: []
}>()

const ReactExcalidraw = applyReactInVue(ExcalidrawWrapper, {
  react: {
    createRoot
  }
})

function handleChange(data: string) {
  emit('change', data)
}

function handleSave() {
  emit('save')
}
</script>

<template>
  <div class="excalidraw-editor">
    <ReactExcalidraw
      :initialData="initialData"
      :readOnly="readOnly"
      :onChange="handleChange"
      :onSave="handleSave"
    />
  </div>
</template>

<style scoped>
.excalidraw-editor {
  width: 100%;
  height: 100%;
}
</style>
