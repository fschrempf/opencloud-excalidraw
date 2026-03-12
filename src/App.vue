<script lang="ts">
import { defineComponent, ref, watch, PropType } from 'vue'
import { Resource } from '@opencloud-eu/web-client'
import { AppConfigObject } from '@opencloud-eu/web-pkg'
import ExcalidrawEditor from './components/ExcalidrawEditor.vue'

export default defineComponent({
  name: 'ExcalidrawApp',
  components: { ExcalidrawEditor },
  props: {
    resource: {
      type: Object as PropType<Resource>,
      required: true
    },
    applicationConfig: {
      type: Object as PropType<AppConfigObject>,
      required: true,
      default: (): AppConfigObject => ({})
    },
    currentContent: {
      type: String,
      required: true
    },
    isReadOnly: {
      type: Boolean,
      required: true
    },
    isDirty: {
      type: Boolean,
      required: true
    }
  },
  emits: ['update:currentContent', 'save', 'close'],
  setup(props, { emit }) {
    const isLocalChange = ref(false)
    const editorContent = ref(props.currentContent || '')

    watch(
      () => props.currentContent,
      (newContent) => {
        if (isLocalChange.value) {
          isLocalChange.value = false
          return
        }
        editorContent.value = newContent || ''
      }
    )

    function handleChange(data: string) {
      isLocalChange.value = true
      emit('update:currentContent', data)
    }

    function handleSave() {
      emit('save')
    }

    return {
      editorContent,
      handleChange,
      handleSave
    }
  }
})
</script>

<template>
  <div class="excalidraw-app">
    <ExcalidrawEditor
      :initialData="editorContent"
      :readOnly="isReadOnly"
      @change="handleChange"
      @save="handleSave"
    />
  </div>
</template>

<style scoped>
.excalidraw-app {
  width: 100%;
  height: 100vh;
  position: relative;
}
</style>
