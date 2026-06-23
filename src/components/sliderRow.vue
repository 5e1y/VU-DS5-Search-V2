<script setup lang="ts">
import { ref, onMounted, onUnmounted, nextTick } from 'vue'
import buttonSlider from './buttonSlider.vue'

const track = ref<HTMLElement | null>(null)
const atStart = ref(true)
const atEnd = ref(false)

function update() {
  const el = track.value
  if (!el) return
  atStart.value = el.scrollLeft <= 1
  atEnd.value = el.scrollLeft + el.clientWidth >= el.scrollWidth - 1
}

function scroll(dir: 1 | -1) {
  const el = track.value
  if (!el) return
  el.scrollBy({ left: dir * el.clientWidth * 0.8, behavior: 'smooth' })
}

let ro: ResizeObserver | null = null
onMounted(async () => {
  await nextTick()
  update()
  if (track.value) {
    ro = new ResizeObserver(update)
    ro.observe(track.value)
  }
})
onUnmounted(() => ro?.disconnect())
</script>

<template>
  <div class="slider-row">
    <buttonSlider
      class="slider-row__ctrl slider-row__ctrl--left"
      direction="left"
      :can-scroll="!atStart"
      @click="scroll(-1)"
    />
    <div ref="track" class="slider-row__track" @scroll="update">
      <slot />
    </div>
    <buttonSlider
      class="slider-row__ctrl slider-row__ctrl--right"
      direction="right"
      :can-scroll="!atEnd"
      @click="scroll(1)"
    />
  </div>
</template>

<style scoped>
.slider-row {
  position: relative;
  width: 100%;
}

.slider-row__track {
  display: flex;
  gap: var(--spacing-extra-small);
  overflow-x: auto;
  overflow-y: clip;
  scrollbar-width: none;
}
.slider-row__track::-webkit-scrollbar {
  display: none;
}

.slider-row__ctrl {
  position: absolute;
  top: 0;
  bottom: 0;
  z-index: 1;
}
.slider-row__ctrl--left {
  left: 0;
}
.slider-row__ctrl--right {
  right: 0;
}
</style>
