<script setup lang="ts">
type SliderButtonState = 'default' | 'rollover'

withDefaults(
  defineProps<{
    state?: SliderButtonState
    disabled?: boolean
  }>(),
  {
    state: 'default',
    disabled: false,
  },
)
</script>

<template>
  <div
    class="input-slider-button"
    :class="[
      `input-slider-button--${state}`,
      disabled && 'input-slider-button--disabled',
    ]"
  >
    <span class="input-slider-button__dot" />
  </div>
</template>

<style scoped>
.input-slider-button {
  position: relative;
  width: 44px;
  height: 44px;
  border-radius: var(--radius-large);
  overflow: hidden;
  flex-shrink: 0;
}

.input-slider-button__dot {
  position: absolute;
  top: 50%;
  left: 50%;
  width: 16px;
  height: 16px;
  border-radius: var(--radius-round);
  background: var(--interactive-primary-background-default);
  transform: translate(-50%, -50%);
  transition:
    width 150ms ease,
    height 150ms ease;
}

/* Rollover — point agrandi (état piloté par le parent ou au survol) */
.input-slider-button--rollover .input-slider-button__dot,
.input-slider-button:not(.input-slider-button--disabled):hover .input-slider-button__dot {
  width: 24px;
  height: 24px;
}

.input-slider-button--disabled {
  opacity: 0.5;
}
</style>
