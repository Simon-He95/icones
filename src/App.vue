<script setup lang='ts'>
import { themeColor } from './store'

const style = computed<any>(() => ({
  '--theme-color': themeColor.value,
}))

onMounted(() => {
  document.addEventListener('keydown', (e) => {
    if ((e.metaKey || e.ctrlKey) && e.key === 'c') {
      const selection = document.getSelection()
      if (!selection || !selection.toString())
        return
      const text = selection.toString()
      window.parent.postMessage({ eventType: 'copy', text }, '*')
    }
  })
})
onUnmounted(() => {
  document.removeEventListener('keydown', (e) => {
    if ((e.metaKey || e.ctrlKey) && e.key === 'c') {
      const selection = document.getSelection()
      if (!selection || !selection.toString())
        return
      const text = selection.toString()
      window.parent.postMessage({ eventType: 'copy', text }, '*')
    }
  })
})
</script>

<template>
  <div class="flex flex-col h-screen overflow-hidden bg-base" :style="style">
    <div class="h-full flex-auto overflow-overlay">
      <RouterView />
    </div>
    <Progress />
  </div>
</template>
