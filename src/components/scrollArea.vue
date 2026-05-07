<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'

const props = withDefaults(
  defineProps<{
    height?: string
    maxHeight?: string
  }>(),
  {},
)

const viewport = ref<HTMLElement | null>(null)
const track = ref<HTMLElement | null>(null)
const scrollTop = ref(0)
const viewportHeight = ref(0)
const contentHeight = ref(0)
const trackHeight = ref(0)

const thumbVisible = computed(() => contentHeight.value > viewportHeight.value)

const thumbHeight = computed(() => {
  if (!thumbVisible.value) return 0
  return Math.max(32, (viewportHeight.value / contentHeight.value) * trackHeight.value)
})

const thumbTop = computed(() => {
  if (!thumbVisible.value) return 0
  const maxScroll = contentHeight.value - viewportHeight.value
  const maxThumbTop = trackHeight.value - thumbHeight.value
  return (scrollTop.value / maxScroll) * maxThumbTop
})

function updateMeasurements() {
  if (!viewport.value || !track.value) return
  viewportHeight.value = viewport.value.clientHeight
  contentHeight.value = viewport.value.scrollHeight
  trackHeight.value = track.value.clientHeight
}

function onScroll() {
  if (!viewport.value) return
  scrollTop.value = viewport.value.scrollTop
}

let isDragging = false
let dragStartY = 0
let dragStartScrollTop = 0

function startDrag(e: MouseEvent) {
  isDragging = true
  dragStartY = e.clientY
  dragStartScrollTop = scrollTop.value
  document.addEventListener('mousemove', onDrag)
  document.addEventListener('mouseup', stopDrag)
  e.preventDefault()
}

function onDrag(e: MouseEvent) {
  if (!isDragging || !viewport.value) return
  const deltaY = e.clientY - dragStartY
  const maxScroll = contentHeight.value - viewportHeight.value
  const maxThumbTop = trackHeight.value - thumbHeight.value
  const scrollDelta = (deltaY / maxThumbTop) * maxScroll
  viewport.value.scrollTop = Math.max(0, Math.min(maxScroll, dragStartScrollTop + scrollDelta))
}

function stopDrag() {
  isDragging = false
  document.removeEventListener('mousemove', onDrag)
  document.removeEventListener('mouseup', stopDrag)
}

let resizeObserver: ResizeObserver | null = null

onMounted(() => {
  updateMeasurements()
  resizeObserver = new ResizeObserver(updateMeasurements)
  if (viewport.value) resizeObserver.observe(viewport.value)
})

onUnmounted(() => {
  resizeObserver?.disconnect()
  document.removeEventListener('mousemove', onDrag)
  document.removeEventListener('mouseup', stopDrag)
})

const containerStyle = computed(() => ({
  height: props.height,
  maxHeight: props.maxHeight,
}))

const thumbStyle = computed(() => ({
  height: `${thumbHeight.value}px`,
  transform: `translateY(${thumbTop.value}px)`,
}))
</script>

<template>
  <div class="scroll-area" :style="containerStyle">
    <div ref="viewport" class="scroll-area__viewport" @scroll="onScroll">
      <slot />
    </div>
    <div ref="track" class="scroll-area__track">
      <div
        v-if="thumbVisible"
        class="scroll-area__thumb"
        :style="thumbStyle"
        @mousedown="startDrag"
      />
    </div>
  </div>
</template>

<style scoped>
.scroll-area {
  position: relative;
  overflow: hidden;
}

.scroll-area__viewport {
  height: 100%;
  overflow-y: auto;
  scrollbar-width: none;
}

.scroll-area__viewport::-webkit-scrollbar {
  display: none;
}

.scroll-area__track {
  position: absolute;
  right: 4px;
  top: 4px;
  bottom: 4px;
  width: 8px;
  pointer-events: none;
}

.scroll-area__thumb {
  position: absolute;
  left: 0;
  width: 100%;
  background: var(--interactive-on-image-background-default);
  border-radius: var(--radius-round);
  transition: background 150ms ease;
  pointer-events: auto;
  cursor: grab;
  user-select: none;
}

.scroll-area__thumb:hover {
  background: var(--interactive-on-image-background-rollover);
}

.scroll-area__thumb:active {
  cursor: grabbing;
}
</style>
