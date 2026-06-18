<script setup lang="ts">
import iconMain from './iconMain.vue'

withDefaults(
  defineProps<{
    modelValue?: string
    placeholder?: string
    open?: boolean
    showIcon?: boolean
    icon?: string
    type?: 'default' | 'range'
    disabled?: boolean
  }>(),
  {
    modelValue: '',
    open: false,
    showIcon: false,
    type: 'default',
    disabled: false,
  },
)

const emit = defineEmits<{
  click: []
}>()
</script>

<template>
  <div
    class="select-tab"
    :class="[
      `select-tab--${type}`,
      { 'select-tab--open': open },
      { 'select-tab--disabled': disabled },
    ]"
    role="button"
    :tabindex="disabled ? -1 : 0"
    :aria-expanded="open"
    :aria-disabled="disabled"
    @click="!disabled && emit('click')"
    @keydown.enter.prevent="!disabled && emit('click')"
    @keydown.space.prevent="!disabled && emit('click')"
  >
    <div class="select-tab__content">
      <iconMain v-if="showIcon && icon" :icon="icon" size="small" />
      <span
        class="select-tab__text medium-200"
        :class="{ 'select-tab__text--placeholder': !modelValue }"
      >
        {{ modelValue || placeholder }}
      </span>
    </div>
    <iconMain
      class="select-tab__chevron"
      :class="{ 'select-tab__chevron--open': open }"
      icon="ChevronDown"
      size="large"
    />
  </div>
</template>

<style scoped>
/* ── Base ────────────────────────────────────────────────── */
.select-tab {
  display: inline-flex;
  align-items: center;
  width: 100%;
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

.select-tab:focus-visible {
  outline: none;
}

/* ── Content ─────────────────────────────────────────────── */
.select-tab__content {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-extra-small);
  flex: 1;
  min-width: 0;
  padding: 0 var(--spacing-extra-small);
}

.select-tab__text {
  flex: 1;
  min-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  color: var(--interactive-text-icon-background-default);
  transition: color 150ms ease;
}

.select-tab__text--placeholder {
  color: var(--text-icon-neutral-light);
}

.select-tab:hover .select-tab__text {
  color: var(--interactive-text-icon-background-rollover);
}

.select-tab__chevron {
  flex-shrink: 0;
  transition: transform 200ms ease;
}

.select-tab__chevron--open {
  transform: rotate(180deg);
}

/* ── Type: default ───────────────────────────────────────── */
.select-tab--default {
  background: var(--interactive-tab-background-active);
}

/* ── Type: range ─────────────────────────────────────────── */
.select-tab--range {
  background: var(--interactive-tab-range-background-rollover);
}

/* ── Open / Focus ────────────────────────────────────────── */
.select-tab--open,
.select-tab:focus-visible {
  background: var(--interactive-tab-range-background-active);
  box-shadow:
    inset 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-active),
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-active);
}

/* ── Disabled ────────────────────────────────────────────── */
.select-tab--disabled {
  opacity: 0.5;
  pointer-events: none;
  cursor: default;
}
</style>
