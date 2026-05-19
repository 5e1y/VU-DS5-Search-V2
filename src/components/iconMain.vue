<script setup lang="ts">
import { computed, ref, watch, onMounted, type Component } from 'vue'
import * as LucideIcons from 'lucide-vue-next'

type IconSize =
  | 'extra-small'
  | 'small'
  | 'medium'
  | 'large'
  | 'extra-large'
  | 'extra-extra-large'

const props = withDefaults(
  defineProps<{
    icon: string
    size?: IconSize
    focus?: boolean
    strokeWidth?: number
  }>(),
  {
    size: 'large',
    focus: false,
  },
)

const SIZE_MAP: Record<IconSize, number> = {
  'extra-small': 12,
  small: 16,
  medium: 20,
  large: 24,
  'extra-large': 32,
  'extra-extra-large': 48,
}

const STROKE_MAP: Record<IconSize, number> = {
  'extra-small': 1,
  small: 1.3,
  medium: 1.67,
  large: 2,
  'extra-large': 2.67,
  'extra-extra-large': 3,
}

const sizeInPx = computed(() => SIZE_MAP[props.size])
const strokeWidth = computed(() => {
  const base = props.strokeWidth !== undefined ? props.strokeWidth : STROKE_MAP[props.size]
  return base / scaleFactor.value
})

const iconComponent = computed(
  () => (LucideIcons as Record<string, unknown>)[props.icon] as Component | undefined,
)

const iconRef = ref<HTMLElement | null>(null)
const scaleFactor = computed(() => sizeInPx.value / 24)

function applyViewBoxScale() {
  const svg = iconRef.value?.querySelector('svg')
  if (!svg) return

  const size = sizeInPx.value
  svg.setAttribute('viewBox', `0 0 ${size} ${size}`)

  let scaleGroup = svg.querySelector<SVGGElement>('g[data-icon-scale]')
  if (!scaleGroup) {
    scaleGroup = document.createElementNS('http://www.w3.org/2000/svg', 'g') as SVGGElement
    scaleGroup.setAttribute('data-icon-scale', '')
    while (svg.firstChild) {
      scaleGroup.appendChild(svg.firstChild)
    }
    svg.appendChild(scaleGroup)
  }

  scaleGroup.setAttribute('transform', `scale(${scaleFactor.value})`)
}

onMounted(applyViewBoxScale)
watch([() => props.icon, sizeInPx], applyViewBoxScale, { flush: 'post' })
</script>

<template>
  <div ref="iconRef" :class="['icon-main', focus && 'icon-main--focus']">
    <component :is="iconComponent" :size="sizeInPx" :stroke-width="strokeWidth" />
  </div>
</template>

<style scoped>
.icon-main {
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

.icon-main--focus {
  border: var(--stroke-active) solid var(--interactive-secondary-stroke-focus);
  border-radius: var(--radius-focus-mode);
}
</style>
