<script setup lang="ts">
import { Check } from 'lucide-vue-next'

withDefaults(
  defineProps<{
    type?: 'check' | 'radio' | 'switch'
    modelValue?: boolean
    disabled?: boolean
  }>(),
  {
    type: 'check',
    modelValue: false,
    disabled: false,
  },
)

const emit = defineEmits<{
  'update:modelValue': [value: boolean]
}>()
</script>

<template>
  <button
    type="button"
    class="toggle-main"
    :class="[
      `toggle-main--${type}`,
      modelValue && 'toggle-main--active',
    ]"
    :disabled="disabled"
    :role="type === 'radio' ? 'radio' : 'checkbox'"
    :aria-checked="modelValue"
    @click="emit('update:modelValue', !modelValue)"
  >
    <Check
      v-if="type === 'check' && modelValue"
      class="toggle-main__check"
      :size="12"
      :stroke-width="2.5"
    />
    <span v-if="type === 'switch'" class="toggle-main__thumb" />
  </button>
</template>

<style scoped>
.toggle-main {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: 0;
  border: none;
  cursor: pointer;
  flex-shrink: 0;
  overflow: hidden;
  transition:
    background-color 150ms ease,
    border-color 150ms ease,
    box-shadow 150ms ease;
}

.toggle-main:focus-visible {
  outline: none;
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

.toggle-main:disabled {
  cursor: not-allowed;
  pointer-events: none;
}

/* ── CHECKBOX ── */
.toggle-main--check {
  width: 20px;
  height: 20px;
  border-radius: 6px;
  background: var(--surface-background);
  border: var(--stroke-passive) solid var(--text-icon-neutral-light);
}

.toggle-main--check:not(:disabled):hover {
  border-color: var(--text-icon-neutral-medium);
}

.toggle-main--check.toggle-main--active {
  background: var(--interactive-switch-background-active);
  border-color: var(--interactive-switch-background-active);
}

.toggle-main--check:disabled {
  background: var(--surface-neutral);
  border-color: transparent;
}

/* ── RADIO ── */
.toggle-main--radio {
  width: 20px;
  height: 20px;
  border-radius: var(--radius-round);
  background: var(--surface-background);
  border: var(--stroke-passive) solid var(--text-icon-neutral-light);
}

.toggle-main--radio::after {
  content: '';
  position: absolute;
  inset: 3px;
  border-radius: var(--radius-round);
  background: var(--interactive-switch-background-active);
  transform: scale(0);
  transition: transform 150ms ease;
}

.toggle-main--radio:not(:disabled):hover {
  border-color: var(--text-icon-neutral-medium);
}

.toggle-main--radio.toggle-main--active {
  border: var(--stroke-active) solid var(--interactive-switch-background-active);
}

.toggle-main--radio.toggle-main--active::after {
  transform: scale(1);
}

.toggle-main--radio:disabled {
  background: var(--surface-neutral);
  border-color: transparent;
}

/* ── SWITCH ── */
.toggle-main--switch {
  width: 32px;
  height: 20px;
  border-radius: var(--radius-round);
  background: var(--interactive-switch-background-default);
}

.toggle-main--switch:not(:disabled):hover {
  background: var(--interactive-switch-background-rollover);
}

.toggle-main--switch.toggle-main--active {
  background: var(--interactive-switch-background-active);
}

.toggle-main--switch:disabled {
  background: var(--surface-neutral);
}

/* Switch thumb */
.toggle-main__thumb {
  position: absolute;
  top: 10%;
  bottom: 10%;
  left: 6.25%;
  width: 50%;
  border-radius: var(--radius-round);
  background: var(--surface-background);
  transition: left 200ms ease;
}

.toggle-main--switch:not(:disabled):hover .toggle-main__thumb {
  left: 12.5%;
}

.toggle-main--switch.toggle-main--active .toggle-main__thumb {
  left: 43.75%;
}

.toggle-main--switch.toggle-main--active:not(:disabled):hover .toggle-main__thumb {
  left: 37.5%;
}

.toggle-main--switch:disabled .toggle-main__thumb {
  background: var(--surface-neutral);
}

/* Checkmark */
.toggle-main__check {
  color: white;
  flex-shrink: 0;
  animation: check-in 150ms ease;
}

@keyframes check-in {
  from { transform: scale(0.4); opacity: 0; }
  to   { transform: scale(1);   opacity: 1; }
}
</style>
