<script setup lang="ts">
import { computed, type Component } from 'vue'
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
const strokeWidth = computed(() => STROKE_MAP[props.size])

const iconComponent = computed(
  () => (LucideIcons as Record<string, unknown>)[props.icon] as Component | undefined,
)
</script>

<template>
  <div :class="['icon-main', focus && 'icon-main--focus']">
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
