<script setup lang="ts">
import { computed } from 'vue'
import { RouterLink, type RouteLocationRaw } from 'vue-router'
import iconMain from './iconMain.vue'

const props = withDefaults(
  defineProps<{
    text: string
    href?: string
    to?: RouteLocationRaw
    showIcon?: boolean
    icon?: string
  }>(),
  {
    showIcon: false,
    icon: 'ChevronRight',
  },
)

const tag = computed(() => {
  if (props.href) return 'a'
  if (props.to) return RouterLink
  return 'button'
})

const linkAttrs = computed(() => {
  if (props.href) return { href: props.href, target: '_blank', rel: 'noopener noreferrer' }
  if (props.to) return { to: props.to }
  return { type: 'button' }
})
</script>

<template>
  <component :is="tag" v-bind="linkAttrs" class="text-link medium-underline-200">
    {{ text }}
    <iconMain v-if="showIcon" :icon="icon" size="medium" />
  </component>
</template>

<style scoped>
.text-link {
  display: inline-flex;
  align-items: center;
  gap: var(--spacing-minuscule);
  vertical-align: baseline;
  color: var(--text-icon-neutral-hard);
  background: none;
  border: none;
  padding: 0;
  cursor: pointer;
  transition: color 150ms ease;
}

.text-link:hover {
  color: var(--text-icon-neutral-medium);
}

.text-link:focus-visible {
  outline: var(--stroke-active) solid var(--interactive-secondary-stroke-focus);
  outline-offset: 2px;
  border-radius: var(--radius-focus-mode);
  color: var(--text-icon-neutral-hard);
}
</style>
