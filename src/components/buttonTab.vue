<script setup lang="ts">
import { computed } from 'vue'

type ButtonTabType = 'default' | 'range'

const props = withDefaults(
  defineProps<{
    label?: string
    type?: ButtonTabType
    active?: boolean
    showBadge?: boolean
    count?: number
    disabled?: boolean
    href?: string
  }>(),
  {
    label: '',
    type: 'default',
    active: false,
    showBadge: false,
    disabled: false,
  },
)

const emit = defineEmits<{
  click: []
}>()

const tag = computed(() => (props.href ? 'a' : 'button'))

const showBadge = computed(
  () => props.type === 'default' && props.showBadge,
)
</script>

<template>
  <component
    :is="tag"
    class="button-tab"
    :class="[
      `button-tab--${type}`,
      active && 'button-tab--active',
      disabled && 'button-tab--disabled',
    ]"
    :type="tag === 'button' ? 'button' : undefined"
    :href="tag === 'a' && !disabled ? href : undefined"
    :disabled="tag === 'button' ? disabled : undefined"
    :aria-disabled="tag === 'a' && disabled ? true : undefined"
    role="tab"
    :aria-selected="active"
    @click="!disabled && emit('click')"
  >
    <span class="button-tab__label medium-200">{{ label }}</span>
    <span v-if="showBadge" class="button-tab__badge semi-bold-100">{{ count }}</span>
  </component>
</template>

<style scoped>
.button-tab {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  border: none;
  border-radius: var(--radius-medium);
  overflow: hidden;
  cursor: pointer;
  user-select: none;
  text-decoration: none;
  background: transparent;
  transition:
    background-color 150ms ease,
    box-shadow 150ms ease,
    color 150ms ease;
}

.button-tab:focus-visible {
  outline: none;
}

/* ── Label ───────────────────────────────────────────────── */
.button-tab__label {
  flex: 1;
  min-width: 0;
  text-align: center;
  word-break: break-word;
  color: var(--interactive-text-icon-background-default);
  transition: color 150ms ease;
}

.button-tab:hover .button-tab__label,
.button-tab--active .button-tab__label,
.button-tab:focus-visible .button-tab__label {
  color: var(--interactive-text-icon-background-rollover);
}

/* ── Badge (type=default uniquement) ─────────────────────── */
.button-tab__badge {
  flex-shrink: 0;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 16px;
  min-height: 16px;
  padding-inline: var(--spacing-minuscule);
  border-radius: var(--radius-round);
  background: var(--surface-notification);
  color: var(--text-icon-neutral-on-dark);
  text-align: center;
}

/* ── Type: default ───────────────────────────────────────── */
.button-tab--default {
  gap: var(--spacing-extra-small);
  min-height: 44px;
  padding: var(--spacing-extra-small) var(--spacing-small);
}

.button-tab--default:hover {
  background: var(--interactive-tab-background-rollover);
}

.button-tab--default.button-tab--active {
  background: var(--interactive-tab-background-active);
}

.button-tab--default:focus-visible {
  background: var(--interactive-tab-background-rollover);
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

/* ── Type: range ─────────────────────────────────────────── */
.button-tab--range {
  gap: 0;
  min-height: 52px;
  padding: var(--spacing-small);
}

.button-tab--range:hover {
  background: var(--interactive-tab-range-background-rollover);
}

.button-tab--range.button-tab--active {
  background: var(--interactive-tab-range-background-active);
}

.button-tab--range:focus-visible {
  background: var(--interactive-tab-range-background-active);
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

/* ── Disabled ────────────────────────────────────────────── */
.button-tab--disabled {
  opacity: 0.5;
  pointer-events: none;
  cursor: default;
}
</style>
