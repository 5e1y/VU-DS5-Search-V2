<script setup lang="ts">
import inputComp from './input.vue'
import selectComp from './select.vue'
import largeTextAreaComp from './largeTextArea.vue'
import buttonMain from './buttonMain.vue'

const props = withDefaults(
  defineProps<{
    // Composant interne
    type?: 'field' | 'select' | 'textArea'

    // Partagé
    modelValue?: string
    placeholder?: string
    state?: 'default' | 'error'
    disabled?: boolean

    // Label
    showLabel?: boolean
    label?: string

    // Info / Error
    showInfotext?: boolean
    infotext?: string
    errorAlert?: string

    // Bouton trailing
    showButton?: boolean
    buttonLabel?: string
    buttonIconLeft?: string
    buttonIconRight?: string

    // Geo select (uniquement pour type=field)
    showGeoSelect?: boolean
    geoValue?: string

    // Select-specific
    selectOpen?: boolean
    selectShowIcon?: boolean
    selectIcon?: string
  }>(),
  {
    type: 'field',
    modelValue: '',
    state: 'default',
    disabled: false,
    showLabel: false,
    showInfotext: false,
    showButton: false,
    showGeoSelect: false,
    geoValue: 'FR+33',
    selectOpen: false,
    selectShowIcon: false,
  },
)

const emit = defineEmits<{
  'update:modelValue': [value: string]
  buttonClick: []
  geoSelectClick: []
  selectClick: []
}>()
</script>

<template>
  <div class="input-field-full">
    <!-- Label (toujours au-dessus) -->
    <span v-if="showLabel && label" class="input-field-full__label">
      {{ label }}
    </span>

    <!-- Ligne principale -->
    <div class="input-field-full__row">
      <!-- Geo select (type=field uniquement) -->
      <selectComp
        v-if="type === 'field' && showGeoSelect"
        class="input-field-full__geo-select"
        :model-value="geoValue"
        :disabled="disabled"
        @click="emit('geoSelectClick')"
      />

      <!-- Input (type=field) -->
      <inputComp
        v-if="type === 'field'"
        class="input-field-full__input"
        :model-value="modelValue"
        :placeholder="placeholder"
        :state="state"
        :disabled="disabled"
        @update:model-value="emit('update:modelValue', $event)"
      />

      <!-- Select (type=select) -->
      <selectComp
        v-if="type === 'select'"
        class="input-field-full__select"
        :model-value="modelValue"
        :placeholder="placeholder ?? 'Sélectionner'"
        :open="selectOpen"
        :show-icon="selectShowIcon"
        :icon="selectIcon"
        :state="state"
        :disabled="disabled"
        @click="emit('selectClick')"
      />

      <!-- LargeTextArea (type=textArea) -->
      <largeTextAreaComp
        v-if="type === 'textArea'"
        class="input-field-full__textarea"
        :model-value="modelValue"
        :placeholder="placeholder"
        :state="state"
        :disabled="disabled"
        @update:model-value="emit('update:modelValue', $event)"
      />

      <!-- Bouton trailing -->
      <buttonMain
        v-if="showButton && type !== 'textArea'"
        type="secondary"
        :label="buttonLabel"
        :icon-left="buttonIconLeft"
        :icon-right="buttonIconRight"
        :disabled="disabled"
        @click="emit('buttonClick')"
      />
    </div>

    <!-- Error alert (uniquement pour field/select) -->
    <span
      v-if="state === 'error' && type !== 'textArea' && errorAlert"
      class="input-field-full__error"
    >
      {{ errorAlert }}
    </span>

    <!-- Infotext -->
    <span v-if="showInfotext && infotext" class="input-field-full__infotext">
      {{ infotext }}
    </span>
  </div>
</template>

<style scoped>
/* ── Wrapper ─────────────────────────────────────────────── */
.input-field-full {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-extra-extra-small);
  width: 100%;
}

/* ── Label ───────────────────────────────────────────────── */
.input-field-full__label {
  font-family: 'Poppins', sans-serif;
  font-size: 14px;
  line-height: 20px;
  font-weight: 500;
  color: var(--text-icon-neutral-hard);
}

/* ── Row ─────────────────────────────────────────────────── */
.input-field-full__row {
  display: flex;
  align-items: stretch;
  gap: var(--spacing-extra-extra-small);
  width: 100%;
}

/* ── Geo select ──────────────────────────────────────────── */
.input-field-full__geo-select {
  width: 118px;
  flex-shrink: 0;
}

/* ── Input / Select ──────────────────────────────────────── */
.input-field-full__input,
.input-field-full__select {
  flex: 1;
  min-width: 0;
}

/* ── TextArea ────────────────────────────────────────────── */
.input-field-full__textarea {
  flex: 1;
  min-width: 0;
}

/* ── Error alert ─────────────────────────────────────────── */
.input-field-full__error {
  font-family: 'Poppins', sans-serif;
  font-size: 12px;
  line-height: 16px;
  font-weight: 400;
  color: var(--text-icon-negative-light);
}

/* ── Infotext ────────────────────────────────────────────── */
.input-field-full__infotext {
  font-family: 'Poppins', sans-serif;
  font-size: 12px;
  line-height: 16px;
  font-weight: 400;
  color: var(--text-icon-neutral-light);
}
</style>
