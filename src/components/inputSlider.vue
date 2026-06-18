<script setup lang="ts">
import { computed, onBeforeUnmount, ref } from 'vue'
import inputSliderButton from './inputSliderButton.vue'
import inputComp from './input.vue'

const props = withDefaults(
  defineProps<{
    modelValue?: [number, number]
    min?: number
    max?: number
    step?: number
    label?: string
    disabled?: boolean
  }>(),
  {
    min: 0,
    max: 100,
    step: 1,
    label: '',
    disabled: false,
  },
)

const emit = defineEmits<{
  'update:modelValue': [value: [number, number]]
}>()

/* Source de vérité = modelValue ; fallback sur les bornes si absent. */
const values = computed<[number, number]>(() => props.modelValue ?? [props.min, props.max])

const trackRef = ref<HTMLElement | null>(null)
const draggingIndex = ref<0 | 1 | null>(null)

/* ── Helpers ─────────────────────────────────────────────── */
function clampToBounds(v: number): number {
  return Math.min(props.max, Math.max(props.min, v))
}

function snap(v: number): number {
  const steps = Math.round((v - props.min) / props.step)
  return clampToBounds(props.min + steps * props.step)
}

function percent(v: number): number {
  const span = props.max - props.min
  if (span <= 0) return 0
  return ((v - props.min) / span) * 100
}

function valueFromPointer(clientX: number): number {
  const el = trackRef.value
  if (!el) return props.min
  const rect = el.getBoundingClientRect()
  const ratio = rect.width > 0 ? (clientX - rect.left) / rect.width : 0
  return snap(props.min + ratio * (props.max - props.min))
}

/* Met à jour une poignée en empêchant le croisement, puis émet. */
function updateHandle(index: 0 | 1, raw: number) {
  const snapped = snap(raw)
  const next: [number, number] = [values.value[0], values.value[1]]
  if (index === 0) next[0] = Math.min(snapped, values.value[1])
  else next[1] = Math.max(snapped, values.value[0])
  if (next[0] !== values.value[0] || next[1] !== values.value[1]) {
    emit('update:modelValue', next)
  }
}

/* ── Drag (souris + tactile via Pointer Events) ──────────── */
function onPointerMove(e: PointerEvent) {
  if (draggingIndex.value === null) return
  updateHandle(draggingIndex.value, valueFromPointer(e.clientX))
}

function stopDragging() {
  draggingIndex.value = null
  window.removeEventListener('pointermove', onPointerMove)
  window.removeEventListener('pointerup', stopDragging)
}

function startDragging(index: 0 | 1) {
  draggingIndex.value = index
  window.addEventListener('pointermove', onPointerMove)
  window.addEventListener('pointerup', stopDragging)
}

function onHandlePointerDown(index: 0 | 1, e: PointerEvent) {
  if (props.disabled) return
  e.stopPropagation()
  /* Pas de .focus() ici : le glissement passe par les listeners window, et
     focaliser au pointeur ferait apparaître l'anneau de focus au relâchement.
     L'anneau reste réservé au focus clavier (Tab). */
  startDragging(index)
}

/* Clic sur la piste : déplace la poignée la plus proche. */
function onTrackPointerDown(e: PointerEvent) {
  if (props.disabled) return
  const v = valueFromPointer(e.clientX)
  const index: 0 | 1 = Math.abs(v - values.value[0]) <= Math.abs(v - values.value[1]) ? 0 : 1
  updateHandle(index, v)
  startDragging(index)
}

/* ── Clavier ─────────────────────────────────────────────── */
function onHandleKeydown(index: 0 | 1, e: KeyboardEvent) {
  if (props.disabled) return
  const current = values.value[index]
  let next: number | null = null
  switch (e.key) {
    case 'ArrowRight':
    case 'ArrowUp':
      next = current + props.step
      break
    case 'ArrowLeft':
    case 'ArrowDown':
      next = current - props.step
      break
    case 'PageUp':
      next = current + props.step * 10
      break
    case 'PageDown':
      next = current - props.step * 10
      break
    case 'Home':
      next = index === 0 ? props.min : values.value[0]
      break
    case 'End':
      next = index === 1 ? props.max : values.value[1]
      break
    default:
      return
  }
  e.preventDefault()
  updateHandle(index, next)
}

/* ── Champs de saisie ────────────────────────────────────── */
function onInputChange(index: 0 | 1, raw: string) {
  const parsed = Number(raw.replace(',', '.'))
  if (raw.trim() === '' || Number.isNaN(parsed)) return
  updateHandle(index, parsed)
}

onBeforeUnmount(stopDragging)
</script>

<template>
  <div
    class="input-slider"
    :class="{ 'input-slider--disabled': disabled }"
  >
    <p v-if="label" class="input-slider__label medium-200">{{ label }}</p>

    <div
      ref="trackRef"
      class="input-slider__track-zone"
      @pointerdown="onTrackPointerDown"
    >
      <span class="input-slider__rail" />
      <span
        class="input-slider__fill"
        :style="{
          left: `${percent(values[0])}%`,
          right: `${100 - percent(values[1])}%`,
        }"
      />

      <button
        v-for="index in ([0, 1] as const)"
        :key="index"
        type="button"
        class="input-slider__handle"
        :class="{ 'input-slider__handle--pressed': draggingIndex === index }"
        :style="{ left: `${percent(values[index])}%` }"
        role="slider"
        :aria-valuemin="index === 0 ? min : values[0]"
        :aria-valuemax="index === 0 ? values[1] : max"
        :aria-valuenow="values[index]"
        :aria-label="index === 0 ? 'Valeur minimale' : 'Valeur maximale'"
        :aria-disabled="disabled || undefined"
        :tabindex="disabled ? -1 : 0"
        @pointerdown="onHandlePointerDown(index, $event)"
        @keydown="onHandleKeydown(index, $event)"
      >
        <inputSliderButton
          :state="draggingIndex === index ? 'rollover' : 'default'"
          :disabled="disabled"
        />
      </button>
    </div>

    <div class="input-slider__inputs">
      <div class="input-slider__field">
        <inputComp
          :model-value="String(values[0])"
          :disabled="disabled"
          @update:model-value="onInputChange(0, $event)"
        />
      </div>
      <div class="input-slider__field">
        <inputComp
          :model-value="String(values[1])"
          :disabled="disabled"
          @update:model-value="onInputChange(1, $event)"
        />
      </div>
    </div>
  </div>
</template>

<style scoped>
.input-slider {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-extra-small);
  padding-block: var(--spacing-small);
  width: 100%;
}

.input-slider__label {
  margin: 0;
  color: var(--text-icon-neutral-hard);
}

/* ── Piste ───────────────────────────────────────────────── */
.input-slider__track-zone {
  position: relative;
  width: 100%;
  height: 44px;
  touch-action: none;
}

.input-slider__rail {
  position: absolute;
  top: 50%;
  left: 0;
  right: 0;
  height: 4px;
  border-radius: var(--radius-round);
  background: var(--stroke-neutral-hard);
  transform: translateY(-50%);
}

.input-slider__fill {
  position: absolute;
  top: 50%;
  height: 4px;
  border-radius: var(--radius-round);
  background: var(--interactive-primary-background-default);
  transform: translateY(-50%);
}

/* ── Poignées ────────────────────────────────────────────── */
.input-slider__handle {
  position: absolute;
  top: 50%;
  padding: 0;
  border: none;
  background: none;
  border-radius: var(--radius-large);
  cursor: grab;
  transform: translate(-50%, -50%);
  transition: box-shadow 150ms ease;
}

.input-slider__handle:active {
  cursor: grabbing;
}

.input-slider__handle:focus-visible {
  outline: none;
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

/* Pressé (glissement actif) = identique au hover : point agrandi seul, sans
   anneau. L'anneau de focus reste réservé au focus clavier. */
.input-slider__handle--pressed.input-slider__handle:focus-visible {
  box-shadow: none;
}

/* ── Champs ──────────────────────────────────────────────── */
.input-slider__inputs {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}

.input-slider__field {
  width: 86px;
}

/* ── Disabled ────────────────────────────────────────────── */
.input-slider--disabled .input-slider__track-zone {
  pointer-events: none;
  opacity: 0.5;
}

.input-slider--disabled .input-slider__handle {
  cursor: not-allowed;
}
</style>
