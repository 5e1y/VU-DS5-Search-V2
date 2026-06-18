<script setup lang="ts">
import { computed } from 'vue'
import iconMain from './iconMain.vue'

const props = withDefaults(
  defineProps<{
    modelValue?: boolean
    color?: string
    image?: string
    label?: string
    count?: number
    disabled?: boolean
  }>(),
  {
    modelValue: false,
    color: '',
    image: '',
    label: '',
    disabled: false,
  },
)

const emit = defineEmits<{
  'update:modelValue': [value: boolean]
}>()

/* Nuancier = donnée produit (couleur, dégradé ou image), pas un token DS. */
const swatchStyle = computed(() =>
  props.image
    ? { backgroundImage: `url("${props.image}")`, backgroundSize: 'cover', backgroundPosition: 'center' }
    : { background: props.color },
)
</script>

<template>
  <component
    :is="disabled ? 'div' : 'button'"
    :type="disabled ? undefined : 'button'"
    class="toggle-color-filter"
    :class="[
      modelValue && 'toggle-color-filter--active',
      disabled && 'toggle-color-filter--disabled',
    ]"
    :role="disabled ? undefined : 'checkbox'"
    :aria-checked="disabled ? undefined : modelValue"
    @click="!disabled && emit('update:modelValue', !modelValue)"
  >
    <span class="toggle-color-filter__group">
      <span class="toggle-color-filter__swatch" :style="swatchStyle" />

      <span
        v-if="!modelValue && count != null"
        class="toggle-color-filter__pastille shadow-small medium-100"
      >{{ count }}</span>

      <span v-else-if="modelValue" class="toggle-color-filter__check">
        <iconMain icon="Check" size="medium" />
      </span>
    </span>

    <span class="toggle-color-filter__label normal-100">{{ label }}</span>
  </component>
</template>

<style scoped>
.toggle-color-filter {
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  gap: var(--spacing-extra-extra-small);
  min-width: 60px;
  padding: var(--spacing-extra-small) var(--spacing-extra-extra-small);
  border: none;
  border-radius: var(--radius-medium);
  background: transparent;
  cursor: pointer;
  transition: box-shadow 150ms ease;
}

.toggle-color-filter:focus-visible {
  outline: none;
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

.toggle-color-filter--disabled {
  cursor: default;
  pointer-events: none;
}

/* ── Color group (ring) ── */
.toggle-color-filter__group {
  position: relative;
  width: 36px;
  height: 36px;
  flex-shrink: 0;
  border-radius: var(--radius-round);
  transition: box-shadow 150ms ease;
}

.toggle-color-filter:not(.toggle-color-filter--disabled):not(.toggle-color-filter--active):hover .toggle-color-filter__group {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-rollover);
}

.toggle-color-filter--active .toggle-color-filter__group {
  box-shadow: inset 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-active);
}

/* ── Swatch (couleur produit) ── */
.toggle-color-filter__swatch {
  position: absolute;
  inset: 0;
  border-radius: var(--radius-round);
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--stroke-neutral-hard);
  background-color: var(--surface-background);
  transition: inset 150ms ease;
}

.toggle-color-filter:not(.toggle-color-filter--disabled):hover .toggle-color-filter__swatch,
.toggle-color-filter--active .toggle-color-filter__swatch {
  inset: var(--spacing-extra-extra-small);
}

/* ── Pastille (compteur) ── */
.toggle-color-filter__pastille {
  position: absolute;
  top: calc(-1 * (var(--spacing-extra-extra-small) + var(--spacing-minuscule)));
  right: calc(-1 * (var(--spacing-extra-extra-small) + var(--spacing-minuscule)));
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: var(--spacing-large);
  padding: var(--spacing-extra-extra-small) var(--spacing-minuscule);
  border-radius: var(--radius-round);
  background: var(--surface-background);
  color: var(--text-icon-neutral-light);
  white-space: nowrap;
}

/* ── Coche (état actif) ── */
.toggle-color-filter__check {
  position: absolute;
  inset: 0;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  color: var(--text-icon-neutral-hard);
}

/* ── Label ── */
.toggle-color-filter__label {
  width: 100%;
  color: var(--interactive-text-icon-background-default);
  text-align: center;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  transition: color 150ms ease;
}

.toggle-color-filter:not(.toggle-color-filter--disabled):hover .toggle-color-filter__label {
  color: var(--interactive-text-icon-background-rollover);
}

.toggle-color-filter--disabled .toggle-color-filter__label {
  color: var(--interactive-text-icon-background-disabled);
}
</style>
