<script setup lang="ts">
import iconMain from './iconMain.vue'

withDefaults(
  defineProps<{
    modelValue?: boolean
    disabled?: boolean
  }>(),
  {
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
    class="toggle-eye"
    :disabled="disabled"
    :aria-label="modelValue ? 'Masquer le mot de passe' : 'Afficher le mot de passe'"
    :aria-pressed="modelValue"
    @click="emit('update:modelValue', !modelValue)"
  >
    <iconMain :icon="modelValue ? 'EyeOff' : 'Eye'" size="large" />
  </button>
</template>

<style scoped>
.toggle-eye {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  width: 48px;
  height: 48px;
  padding: var(--spacing-small);
  border: none;
  background: transparent;
  border-radius: var(--radius-medium);
  cursor: pointer;
  color: var(--text-icon-neutral-hard);
  transition: background-color 150ms ease;
}

.toggle-eye:hover {
  background: var(--interactive-ghost-background-rollover);
}

.toggle-eye:focus-visible {
  outline: none;
  background: var(--interactive-ghost-background-rollover);
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

.toggle-eye:disabled {
  opacity: 0.5;
  cursor: not-allowed;
  pointer-events: none;
}
</style>
