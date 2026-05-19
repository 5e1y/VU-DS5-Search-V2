<script setup lang="ts">
import toggleMain from './toggleMain.vue'

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
    <toggleMain
      tag="div"
      :type="toggleType"
      :model-value="modelValue"
      :disabled="disabled"
    />

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
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-default);
  align-items: center;
}

.toggle-choice--stroke:not(.toggle-choice--disabled):hover {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-rollover);
}

.toggle-choice--stroke.toggle-choice--active {
  box-shadow: inset 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-active);
}

.toggle-choice--stroke.toggle-choice--active:not(.toggle-choice--disabled):hover {
  box-shadow: inset 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-rollover);
}

.toggle-choice--stroke:focus-visible {
  outline: none;
  box-shadow:
    inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-rollover),
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

/* Inner toggle — visuel seulement, tous les états pilotés par le parent */
.toggle-choice :deep(.toggle-main) {
  pointer-events: none;
}

.toggle-choice:not(.toggle-choice--disabled):not(.toggle-choice--active):hover :deep(.toggle-main--check) {
  border-color: var(--text-icon-neutral-medium);
}

.toggle-choice--active:not(.toggle-choice--disabled):hover :deep(.toggle-main--check) {
  background: var(--interactive-primary-background-rollover);
}

.toggle-choice:not(.toggle-choice--disabled):not(.toggle-choice--active):hover :deep(.toggle-main--radio) {
  border-color: var(--text-icon-neutral-medium);
}

/* Disabled states */
.toggle-choice--disabled {
  cursor: default;
  pointer-events: none;
}

.toggle-choice--stroke.toggle-choice--disabled {
  background: var(--surface-neutral);
  box-shadow: none;
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
