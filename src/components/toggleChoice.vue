<script setup lang="ts">
import { Check } from 'lucide-vue-next'

withDefaults(
  defineProps<{
    toggleType?: 'check' | 'radio'
    modelValue?: boolean
    stroke?: boolean
    label?: string
    description?: string
    disabled?: boolean
  }>(),
  {
    toggleType: 'check',
    modelValue: false,
    stroke: false,
    label: '',
    disabled: false,
  },
)

const emit = defineEmits<{
  'update:modelValue': [value: boolean]
}>()
</script>

<template>
  <component
    :is="disabled ? 'div' : 'button'"
    type="button"
    class="toggle-choice"
    :class="[
      stroke && 'toggle-choice--stroke',
      modelValue && 'toggle-choice--active',
      disabled && 'toggle-choice--disabled',
    ]"
    :aria-pressed="disabled ? undefined : modelValue"
    @click="!disabled && emit('update:modelValue', !modelValue)"
  >
    <!-- Toggle visual (check or radio) -->
    <div
      class="toggle-choice__toggle"
      :class="[
        `toggle-choice__toggle--${toggleType}`,
        modelValue && 'toggle-choice__toggle--active',
        disabled && 'toggle-choice__toggle--disabled',
      ]"
    >
      <Check
        v-if="toggleType === 'check' && modelValue && !disabled"
        class="toggle-choice__check"
        :size="12"
        :stroke-width="2.5"
      />
      <span
        v-if="toggleType === 'radio' && modelValue"
        class="toggle-choice__dot"
      />
    </div>

    <!-- Text area -->
    <div class="toggle-choice__text">
      <p class="toggle-choice__label normal-200">{{ label }}</p>
      <p v-if="description" class="toggle-choice__desc normal-100">{{ description }}</p>
    </div>
  </component>
</template>

<style scoped>
.toggle-choice {
  display: flex;
  align-items: flex-start;
  gap: var(--spacing-extra-small);
  width: 100%;
  padding: var(--spacing-small) 0;
  background: transparent;
  border: none;
  cursor: pointer;
  text-align: left;
  transition:
    background-color 150ms ease,
    border-color 150ms ease,
    box-shadow 150ms ease;
}

/* Stroke variant — pill container */
.toggle-choice--stroke {
  padding: var(--spacing-small);
  border-radius: var(--radius-small);
  border: var(--stroke-passive) solid var(--interactive-secondary-stroke-default);
  align-items: center;
}

.toggle-choice--stroke:not(.toggle-choice--disabled):hover {
  border-color: var(--interactive-secondary-stroke-rollover);
}

.toggle-choice--stroke.toggle-choice--active {
  border-width: var(--stroke-active);
  border-color: var(--interactive-secondary-stroke-active);
}

.toggle-choice--stroke:focus-visible {
  outline: none;
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

/* Disabled states */
.toggle-choice--disabled {
  cursor: default;
  pointer-events: none;
}

.toggle-choice--stroke.toggle-choice--disabled {
  background: var(--surface-neutral);
  border-color: transparent;
}

/* ── Toggle visual ── */
.toggle-choice__toggle {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 20px;
  height: 20px;
  flex-shrink: 0;
  overflow: hidden;
  transition:
    background-color 150ms ease,
    border-color 150ms ease;
}

/* Checkbox visual */
.toggle-choice__toggle--check {
  border-radius: 6px;
  background: var(--surface-background);
  border: var(--stroke-passive) solid var(--text-icon-neutral-light);
}

.toggle-choice:not(.toggle-choice--disabled):hover .toggle-choice__toggle--check {
  border-color: var(--text-icon-neutral-medium);
}

.toggle-choice__toggle--check.toggle-choice__toggle--active {
  background: var(--interactive-switch-background-active);
  border-color: var(--interactive-switch-background-active);
}

.toggle-choice__toggle--check.toggle-choice__toggle--disabled {
  background: var(--surface-neutral);
  border-color: transparent;
}

/* Radio visual */
.toggle-choice__toggle--radio {
  border-radius: var(--radius-round);
  background: var(--surface-background);
  border: var(--stroke-passive) solid var(--text-icon-neutral-light);
}

.toggle-choice:not(.toggle-choice--disabled):hover .toggle-choice__toggle--radio {
  border-color: var(--text-icon-neutral-medium);
}

.toggle-choice__toggle--radio.toggle-choice__toggle--active {
  border-width: var(--stroke-active);
  border-color: var(--interactive-switch-background-active);
}

.toggle-choice__toggle--radio.toggle-choice__toggle--disabled {
  background: var(--surface-neutral);
  border-color: transparent;
}

/* Checkmark icon */
.toggle-choice__check {
  color: white;
  flex-shrink: 0;
  animation: check-in 150ms ease;
}

/* Radio dot */
.toggle-choice__dot {
  position: absolute;
  inset: 3px;
  border-radius: var(--radius-round);
  background: var(--interactive-switch-background-active);
  animation: dot-in 150ms ease;
}

@keyframes check-in {
  from { transform: scale(0.4); opacity: 0; }
  to   { transform: scale(1);   opacity: 1; }
}

@keyframes dot-in {
  from { transform: scale(0); }
  to   { transform: scale(1); }
}

/* ── Text area ── */
.toggle-choice__text {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: var(--spacing-minuscule);
  min-width: 0;
}

.toggle-choice__label {
  margin: 0;
  color: var(--interactive-text-icon-background-default);
  transition: color 150ms ease;
}

.toggle-choice:not(.toggle-choice--disabled):hover .toggle-choice__label {
  color: var(--interactive-text-icon-background-rollover);
}

.toggle-choice--disabled .toggle-choice__label {
  color: var(--interactive-text-icon-background-disabled);
}

.toggle-choice__desc {
  margin: 0;
  color: var(--text-icon-neutral-light);
}

.toggle-choice--disabled .toggle-choice__desc {
  color: var(--interactive-text-icon-background-disabled);
}
</style>
