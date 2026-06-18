<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(
  defineProps<{
    page: number | string
    active?: boolean
    disabled?: boolean
    href?: string
  }>(),
  {
    active: false,
    disabled: false,
  },
)

const tag = computed(() => (props.href ? 'a' : 'button'))

const classes = computed(() => ['btn-pagination', props.active && 'btn-pagination--active'])
</script>

<template>
  <component
    :is="tag"
    :class="classes"
    :disabled="tag === 'button' ? disabled : undefined"
    :aria-disabled="tag === 'a' && disabled ? true : undefined"
    :aria-current="active ? 'page' : undefined"
    :href="href"
  >
    <span class="btn-pagination__label medium-300">{{ page }}</span>
  </component>
</template>

<style scoped>
.btn-pagination {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: none;
  background: none;
  cursor: pointer;
  text-decoration: none;
  border-radius: var(--radius-medium);
  padding: var(--spacing-small);
  overflow: hidden;
  color: var(--interactive-text-icon-background-default);
  transition:
    background-color 150ms ease,
    box-shadow 150ms ease,
    color 150ms ease;
}

.btn-pagination__label {
  padding-inline: var(--spacing-extra-small);
}

.btn-pagination:hover {
  background: var(--interactive-ghost-background-rollover);
  color: var(--interactive-text-icon-background-rollover);
}

.btn-pagination--active {
  box-shadow: inset 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-active);
  color: var(--interactive-text-icon-background-rollover);
}

.btn-pagination:focus-visible {
  outline: none;
  background: var(--interactive-ghost-background-rollover);
  color: var(--interactive-text-icon-background-rollover);
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

.btn-pagination:disabled,
.btn-pagination[aria-disabled='true'] {
  cursor: not-allowed;
  pointer-events: none;
  opacity: 0.5;
}
</style>
