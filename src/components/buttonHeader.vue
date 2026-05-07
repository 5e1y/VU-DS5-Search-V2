<script setup lang="ts">
import { computed } from 'vue'
import iconBadge from './iconBadge.vue'

type BadgeVariant = 'number' | 'check' | 'dot' | 'none'

const props = withDefaults(
  defineProps<{
    label?: string
    icon?: string
    badge?: BadgeVariant
    count?: number
    href?: string
    disabled?: boolean
  }>(),
  {
    badge: 'none',
    count: 0,
    disabled: false,
  },
)

const tag = computed(() => (props.href ? 'a' : 'button'))
</script>

<template>
  <component
    :is="tag"
    :href="href"
    :disabled="tag === 'button' ? disabled : undefined"
    :aria-disabled="disabled"
    :class="['btn-header', disabled && 'btn-header--disabled']"
  >
    <span v-if="label" class="btn-header__label medium-200">{{ label }}</span>
    <iconBadge v-if="icon" :icon="icon" :badge="badge" :count="count" />
  </component>
</template>

<style scoped>
.btn-header {
  display: inline-flex;
  align-items: center;
  overflow: hidden;
  padding: var(--spacing-extra-small);
  border-radius: var(--radius-round);
  background: transparent;
  border: none;
  cursor: pointer;
  text-decoration: none;
  transition:
    background-color 150ms ease,
    box-shadow 150ms ease;
}

.btn-header__label {
  height: 32px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 0 var(--spacing-extra-extra-small);
  color: var(--interactive-text-icon-background-default);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.btn-header:hover,
.btn-header:focus-visible {
  background: var(--interactive-ghost-background-rollover);
}

.btn-header:hover .btn-header__label,
.btn-header:focus-visible .btn-header__label {
  color: var(--interactive-text-icon-background-rollover);
}

.btn-header:focus-visible {
  outline: none;
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

.btn-header--disabled {
  cursor: default;
  pointer-events: none;
}

.btn-header--disabled .btn-header__label {
  color: var(--interactive-text-icon-background-disabled);
}
</style>
