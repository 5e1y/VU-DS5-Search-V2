<script setup lang="ts">
import { ref } from 'vue'
import iconMain from './iconMain.vue'

const props = withDefaults(
  defineProps<{
    modelValue?: string
    placeholder?: string
    state?: 'default' | 'error'
    disabled?: boolean
    minHeight?: number
    maxHeight?: number
    name?: string
    id?: string
  }>(),
  {
    modelValue: '',
    state: 'default',
    disabled: false,
    minHeight: 160,
  },
)

const emit = defineEmits<{
  'update:modelValue': [value: string]
}>()

const height = ref(props.minHeight)

function onResizeStart(e: MouseEvent) {
  const startY = e.clientY
  const startH = height.value

  const onMove = (e: MouseEvent) => {
    const delta = e.clientY - startY
    const newH = Math.max(props.minHeight, startH + delta)
    height.value = props.maxHeight !== undefined ? Math.min(newH, props.maxHeight) : newH
  }

  const onUp = () => {
    window.removeEventListener('mousemove', onMove)
    window.removeEventListener('mouseup', onUp)
  }

  window.addEventListener('mousemove', onMove)
  window.addEventListener('mouseup', onUp)
}
</script>

<template>
  <div
    class="large-text-area"
    :class="[`large-text-area--${state}`, { 'large-text-area--disabled': disabled }]"
    :style="{ height: height + 'px' }"
  >
    <textarea
      class="large-text-area__field"
      :value="modelValue"
      :placeholder="placeholder"
      :disabled="disabled"
      :name="name"
      :id="id"
      @input="emit('update:modelValue', ($event.target as HTMLTextAreaElement).value)"
    />
    <iconMain
      class="large-text-area__resize"
      icon="ChevronsUpDown"
      size="small"
      @mousedown.prevent="onResizeStart"
    />
  </div>
</template>

<style scoped>
/* ── Base ────────────────────────────────────────────────── */
.large-text-area {
  position: relative;
  display: flex;
  width: 100%;
  border-radius: var(--radius-small);
  background: var(--interactive-input-background-default);
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-default);
  overflow: hidden;
  transition:
    background-color 150ms ease,
    box-shadow 150ms ease;
}

.large-text-area:hover {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-rollover);
}

.large-text-area:focus-within {
  background: var(--surface-background);
  box-shadow:
    inset 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-active),
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-active);
}

/* ── Error ───────────────────────────────────────────────── */
.large-text-area--error {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-negative);
}

.large-text-area--error:hover {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-negative);
}

.large-text-area--error:focus-within {
  background: var(--surface-background);
  box-shadow:
    inset 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-negative),
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-negative);
}

/* ── Disabled ────────────────────────────────────────────── */
.large-text-area--disabled {
  opacity: 0.5;
  pointer-events: none;
}

/* ── Textarea ────────────────────────────────────────────── */
.large-text-area__field {
  flex: 1;
  width: 100%;
  height: 100%;
  padding: var(--spacing-small);
  background: transparent;
  border: none;
  outline: none;
  resize: none;
  font-family: 'Poppins', sans-serif;
  font-size: 14px;
  line-height: 20px;
  letter-spacing: 0;
  font-weight: 400;
  color: var(--text-icon-neutral-hard);
}

.large-text-area__field::placeholder {
  color: var(--text-icon-neutral-light);
}

/* ── Resize handle ───────────────────────────────────────── */
.large-text-area__resize {
  position: absolute;
  bottom: 3px;
  right: 3px;
  cursor: ns-resize;
  color: var(--text-icon-neutral-light);
  flex-shrink: 0;
}
</style>
