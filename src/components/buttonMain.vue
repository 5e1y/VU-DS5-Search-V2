<script setup lang="ts">
import { computed } from 'vue'
import iconMain from './iconMain.vue'

type ButtonType = 'primary' | 'secondary' | 'tertiary' | 'text' | 'ghost' | 'primary-on-picture'
type ButtonSize = 'default' | 'small'

const props = withDefaults(
  defineProps<{
    type?: ButtonType
    size?: ButtonSize
    iconLeft?: string
    iconRight?: string
    label?: string
    disabled?: boolean
    href?: string
  }>(),
  {
    type: 'primary',
    size: 'default',
    disabled: false,
  },
)

const tag = computed(() => (props.href ? 'a' : 'button'))

type IconSize = 'medium' | 'large'
const iconSize = computed<IconSize>(() => (props.size === 'small' ? 'medium' : 'large'))

const textClass = computed(() => {
  if (props.size === 'small') return 'medium-200'
  return props.type === 'primary' ? 'semi-bold-300' : 'medium-300'
})

const btnClasses = computed(() => ['btn', `btn--${props.type}`, `btn--${props.size}`])
</script>

<template>
  <component
    :is="tag"
    :class="btnClasses"
    :disabled="tag === 'button' ? disabled : undefined"
    :aria-disabled="tag === 'a' && disabled ? true : undefined"
    :href="href"
  >
    <iconMain v-if="iconLeft" :icon="iconLeft" :size="iconSize" />
    <span v-if="label" :class="['btn__label', textClass]">{{ label }}</span>
    <iconMain v-if="iconRight" :icon="iconRight" :size="iconSize" />
  </component>
</template>

<style scoped>
/* ── Base ────────────────────────────────────────────────── */
.btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: none;
  background: none;
  cursor: pointer;
  text-decoration: none;
  border-radius: var(--radius-large);
  padding: var(--spacing-small);
  overflow: hidden;
  transition:
    background-color 150ms ease,
    border-color 150ms ease,
    color 150ms ease,
    box-shadow 150ms ease;
}

.btn:focus-visible {
  outline: none;
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

.btn:disabled,
.btn[aria-disabled='true'] {
  cursor: not-allowed;
  pointer-events: none;
}

/* ── Label padding ───────────────────────────────────────── */
.btn--default .btn__label {
  padding-inline: var(--spacing-extra-small);
}

.btn--small .btn__label {
  padding-inline: var(--spacing-extra-extra-small);
}

/* ── Primary ─────────────────────────────────────────────── */
.btn--primary {
  background: var(--interactive-primary-background-default);
  color: var(--text-icon-neutral-on-dark);
}

.btn--primary:hover {
  background: var(--interactive-primary-background-rollover);
}

.btn--primary:focus-visible {
  background: var(--interactive-on-image-background-rollover);
}

.btn--primary:disabled,
.btn--primary[aria-disabled='true'] {
  background: var(--interactive-primary-background-disabled);
  color: var(--text-icon-neutral-on-dark);
  opacity: 0.5;
}

/* ── Secondary ───────────────────────────────────────────── */
.btn--secondary {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-default);
  color: var(--interactive-text-icon-background-default);
}

.btn--secondary:hover {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-rollover);
}

.btn--secondary:focus-visible {
  box-shadow:
    inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-rollover),
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

.btn--secondary:disabled,
.btn--secondary[aria-disabled='true'] {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-disabled);
  color: var(--interactive-text-icon-background-disabled);
}

/* ── Tertiary ────────────────────────────────────────────── */
.btn--tertiary {
  background: var(--interactive-tertiary-background-default);
  color: var(--interactive-text-icon-background-default);
}

.btn--tertiary:hover {
  background: var(--interactive-tertiary-background-rollover);
}

.btn--tertiary:focus-visible {
  background: var(--interactive-tertiary-background-rollover);
}

.btn--tertiary:disabled,
.btn--tertiary[aria-disabled='true'] {
  color: var(--interactive-text-icon-background-disabled);
}

/* ── Ghost ───────────────────────────────────────────────── */
.btn--ghost {
  background: var(--interactive-ghost-background-default);
  color: var(--interactive-text-icon-background-default);
}

.btn--ghost:hover {
  background: var(--interactive-ghost-background-rollover);
}

.btn--ghost:focus-visible {
  background: var(--interactive-ghost-background-rollover);
}

.btn--ghost:disabled,
.btn--ghost[aria-disabled='true'] {
  color: var(--interactive-text-icon-background-disabled);
}

/* ── Text ────────────────────────────────────────────────── */
.btn--text {
  border-radius: 0;
  border-bottom: var(--stroke-active) solid currentColor;
  padding: var(--spacing-extra-small) var(--spacing-none);
  gap: var(--spacing-extra-small);
  color: var(--interactive-text-icon-background-default);
  transition:
    background-color 150ms ease,
    border-color 150ms ease,
    color 150ms ease,
    gap 150ms ease;
}

.btn--text .btn__label {
  padding-inline: 0;
}

.btn--text:hover {
  color: var(--interactive-text-icon-background-rollover);
  gap: var(--spacing-medium);
}

.btn--text:focus-visible:hover {
  background: none;
}

.btn--text:disabled,
.btn--text[aria-disabled='true'] {
  color: var(--interactive-text-icon-background-disabled);
}

/* ── Primary on picture ──────────────────────────────────── */
.btn--primary-on-picture {
  background: var(--interactive-on-image-background-default);
  backdrop-filter: blur(var(--effect-bg-blur-blur-default));
  color: var(--text-icon-neutral-on-dark);
  box-shadow:
    inset 0 0 0 var(--stroke-passive) var(--interactive-on-image-stroke-default),
    var(--effect-shadow-position-none)
    var(--effect-shadow-position-small)
    var(--effect-bg-blur-blur-small)
    var(--effect-shadow-spread-none)
    var(--shadow-neutral);
}

.btn--primary-on-picture:hover {
  background: var(--interactive-on-image-background-rollover);
}

.btn--primary-on-picture:focus-visible {
  background: var(--interactive-primary-background-rollover);
}
</style>
