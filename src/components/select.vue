<script setup lang="ts">
import iconMain from './iconMain.vue'

const props = withDefaults(
  defineProps<{
    modelValue?: string
    placeholder?: string
    open?: boolean
    showIcon?: boolean
    icon?: string
    state?: 'default' | 'error'
    type?: 'default' | 'range'
    disabled?: boolean
    name?: string
    id?: string
  }>(),
  {
    modelValue: '',
    state: 'default',
    type: 'default',
    open: false,
    showIcon: false,
    disabled: false,
  },
)

const emit = defineEmits<{
  click: []
}>()
</script>

<template>
  <div
    class="select"
    :class="[
      `select--${type}`,
      `select--${state}`,
      { 'select--open': open },
      { 'select--disabled': disabled },
    ]"
    role="button"
    :tabindex="disabled ? -1 : 0"
    :aria-expanded="open"
    :aria-disabled="disabled"
    :id="id"
    @click="!disabled && emit('click')"
    @keydown.enter.prevent="!disabled && emit('click')"
    @keydown.space.prevent="!disabled && emit('click')"
  >
    <div class="select__content">
      <iconMain v-if="showIcon && icon" :icon="icon" size="small" />
      <span class="select__text" :class="{ 'select__text--placeholder': !modelValue }">
        {{ modelValue || placeholder }}
      </span>
    </div>
    <iconMain
      class="select__chevron"
      :class="{ 'select__chevron--open': open }"
      icon="ChevronDown"
      size="large"
    />
  </div>
</template>

<style scoped>
/* ── Base ────────────────────────────────────────────────── */
.select {
  display: inline-flex;
  align-items: center;
  width: 100%;
  height: 48px;
  padding: var(--spacing-small);
  border-radius: var(--radius-small);
  gap: var(--spacing-extra-small);
  overflow: hidden;
  cursor: pointer;
  user-select: none;
  transition:
    background-color 150ms ease,
    box-shadow 150ms ease;
}

.select:focus-visible {
  outline: none;
}

/* ── Content ─────────────────────────────────────────────── */
.select__content {
  display: flex;
  align-items: center;
  gap: var(--spacing-extra-small);
  flex: 1;
  min-width: 0;
  padding: 0 var(--spacing-extra-small);
}

.select__text {
  flex: 1;
  min-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  font-family: 'Poppins', sans-serif;
  font-size: 14px;
  line-height: 20px;
  letter-spacing: 0;
  font-weight: 500;
  color: var(--interactive-text-icon-background-default);
}

.select__text--placeholder {
  font-weight: 400;
  color: var(--text-icon-neutral-light);
}

.select__chevron {
  flex-shrink: 0;
  transition: transform 200ms ease;
}

.select__chevron--open {
  transform: rotate(180deg);
}

/* ── Type: default ───────────────────────────────────────── */
.select--default {
  background: var(--surface-background);
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-default);
}

.select--default:hover {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-rollover);
}

.select--default.select--open,
.select--default:focus-visible {
  box-shadow:
    inset 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-active),
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-active);
}

/* ── Type: range ─────────────────────────────────────────── */
.select--range {
  background: transparent;
  box-shadow: none;
}

.select--range:hover {
  background: var(--interactive-tab-range-background-rollover);
  box-shadow: none;
}

.select--range.select--open,
.select--range:focus-visible {
  background: var(--interactive-tab-range-background-active);
  box-shadow:
    inset 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-active),
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-active);
}

/* ── Error (uniquement type=default) ────────────────────── */
.select--default.select--error {
  background: var(--interactive-input-background-default);
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-negative);
}

.select--default.select--error:hover {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-negative);
}

.select--default.select--error.select--open,
.select--default.select--error:focus-visible {
  box-shadow:
    inset 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-negative),
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-negative);
}

/* ── Disabled ────────────────────────────────────────────── */
.select--disabled {
  opacity: 0.5;
  pointer-events: none;
  cursor: default;
}
</style>
