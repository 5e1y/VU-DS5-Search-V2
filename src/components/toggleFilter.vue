<script setup lang="ts">
import toggleMain from './toggleMain.vue'

withDefaults(
  defineProps<{
    modelValue?: boolean
    label?: string
    count?: number
    disabled?: boolean
  }>(),
  {
    modelValue: false,
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
    :type="disabled ? undefined : 'button'"
    class="toggle-filter"
    :class="[
      modelValue && 'toggle-filter--active',
      disabled && 'toggle-filter--disabled',
    ]"
    :role="disabled ? undefined : 'checkbox'"
    :aria-checked="disabled ? undefined : modelValue"
    @click="!disabled && emit('update:modelValue', !modelValue)"
  >
    <toggleMain
      tag="div"
      type="check"
      :model-value="modelValue"
      :disabled="disabled"
    />

    <span class="toggle-filter__label normal-200">{{ label }}</span>
    <span v-if="count != null" class="toggle-filter__count normal-200">{{ count }}</span>
  </component>
</template>

<style scoped>
.toggle-filter {
  display: inline-flex;
  align-items: center;
  gap: var(--spacing-extra-small);
  padding-block: var(--spacing-extra-small);
  background: transparent;
  border: none;
  cursor: pointer;
  text-align: left;
  transition:
    background-color 150ms ease,
    box-shadow 150ms ease;
}

.toggle-filter:focus-visible {
  outline: none;
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

/* Inner toggle — visuel seulement, tous les états pilotés par le parent */
.toggle-filter :deep(.toggle-main) {
  pointer-events: none;
}

.toggle-filter:not(.toggle-filter--disabled):not(.toggle-filter--active):hover :deep(.toggle-main--check) {
  border-color: var(--text-icon-neutral-medium);
}

.toggle-filter--active:not(.toggle-filter--disabled):hover :deep(.toggle-main--check) {
  background: var(--interactive-primary-background-rollover);
}

/* ── Label ── */
.toggle-filter__label {
  color: var(--interactive-text-icon-background-default);
  white-space: nowrap;
  transition: color 150ms ease;
}

.toggle-filter:not(.toggle-filter--disabled):hover .toggle-filter__label {
  color: var(--interactive-text-icon-background-rollover);
}

/* ── Count ── */
.toggle-filter__count {
  color: var(--text-icon-neutral-light);
  transition: color 150ms ease;
}

.toggle-filter:not(.toggle-filter--disabled):hover .toggle-filter__count {
  color: var(--text-icon-neutral-hard);
}

/* ── Disabled ── */
.toggle-filter--disabled {
  cursor: default;
  pointer-events: none;
}

.toggle-filter--disabled .toggle-filter__label,
.toggle-filter--disabled .toggle-filter__count {
  color: var(--interactive-text-icon-background-disabled);
}
</style>
