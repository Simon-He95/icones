<script setup lang='ts'>
import { themeColor } from './store'

const style = computed<any>(() => ({
  '--theme-color': themeColor.value,
}))
let text = ''

onMounted(() => {
  document.addEventListener('keydown', (e) => {
    if ((e.metaKey || e.ctrlKey) && e.key === 'c')
      window.parent.postMessage({ eventType: 'copy', text }, '*')
  })
  document.addEventListener('selectionchange', () => {
    const selection = document.getSelection()!
    if (!selection || !selection.toString())
      return
    text = selection.toString()
  })
})
onUnmounted(() => {
  document.removeEventListener('keydown', (e) => {
    if ((e.metaKey || e.ctrlKey) && e.key === 'c')
      window.parent.postMessage({ eventType: 'copy', text }, '*')
  })
  document.removeEventListener('selectionchange', () => {
    const selection = document.getSelection()!
    if (!selection || !selection.toString())
      return
    text = selection.toString()
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
