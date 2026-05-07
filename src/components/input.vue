<script setup lang="ts">
import { ref, computed } from 'vue'
import toggleEye from './toggleEye.vue'

const props = withDefaults(
  defineProps<{
    modelValue?: string
    placeholder?: string
    password?: boolean
    state?: 'default' | 'error'
    disabled?: boolean
    name?: string
    id?: string
  }>(),
  {
    modelValue: '',
    state: 'default',
    password: false,
    disabled: false,
  },
)

const emit = defineEmits<{
  'update:modelValue': [value: string]
}>()

const isVisible = ref(false)

const inputType = computed(() => {
  if (!props.password) return 'text'
  return isVisible.value ? 'text' : 'password'
})
</script>

<template>
  <div
    class="input"
    :class="[`input--${state}`, { 'input--disabled': disabled }]"
  >
    <input
      class="input__field"
      :type="inputType"
      :value="modelValue"
      :placeholder="placeholder"
      :disabled="disabled"
      :name="name"
      :id="id"
      @input="emit('update:modelValue', ($event.target as HTMLInputElement).value)"
    />
    <toggleEye v-if="password" v-model="isVisible" :disabled="disabled" />
  </div>
</template>

<style scoped>
/* ── Base ────────────────────────────────────────────────── */
.input {
  display: inline-flex;
  align-items: center;
  width: 100%;
  height: 48px;
  border-radius: var(--radius-small);
  background: var(--interactive-input-background-default);
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-default);
  overflow: hidden;
  transition:
    background-color 150ms ease,
    box-shadow 150ms ease;
}

.input:hover {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--text-icon-neutral-hard);
}

.input:focus-within {
  background: var(--surface-background);
  box-shadow:
    inset 0 0 0 var(--stroke-active) var(--text-icon-neutral-hard),
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-active);
}

/* ── Error ───────────────────────────────────────────────── */
.input--error {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-negative);
}

.input--error:hover {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-negative);
}

.input--error:focus-within {
  background: var(--surface-background);
  box-shadow:
    inset 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-negative),
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-negative);
}

/* ── Disabled ────────────────────────────────────────────── */
.input--disabled {
  opacity: 0.5;
  pointer-events: none;
}

/* ── Native input ────────────────────────────────────────── */
.input__field {
  flex: 1;
  min-width: 0;
  height: 100%;
  padding: var(--spacing-small);
  background: transparent;
  border: none;
  outline: none;
  font-family: 'Poppins', sans-serif;
  font-size: 14px;        /* font-size/200  */
  line-height: 20px;      /* line-height/200 */
  letter-spacing: 0;
  font-weight: 500;       /* filled — text-icon/neutral/hard */
  color: var(--text-icon-neutral-hard);
}

.input__field::placeholder {
  font-weight: 400;       /* placeholder — text-icon/neutral/light @ 0.61 */
  color: var(--text-icon-neutral-light);
  opacity: 0.61;
}
</style>
