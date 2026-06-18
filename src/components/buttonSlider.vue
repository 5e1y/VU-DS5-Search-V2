<script setup lang="ts">
import { computed } from 'vue'
import buttonMain from './buttonMain.vue'

type Direction = 'left' | 'right' | 'top' | 'bottom'

const props = withDefaults(
  defineProps<{
    direction?: Direction
    canScroll?: boolean
    disabled?: boolean
  }>(),
  {
    direction: 'left',
    canScroll: true,
    disabled: false,
  },
)

const emit = defineEmits<{
  click: [event: MouseEvent]
}>()

const ICON: Record<Direction, string> = {
  left: 'ChevronLeft',
  right: 'ChevronRight',
  top: 'ChevronUp',
  bottom: 'ChevronDown',
}

const LABEL: Record<Direction, string> = {
  left: 'Précédent',
  right: 'Suivant',
  top: 'Précédent',
  bottom: 'Suivant',
}

const icon = computed(() => ICON[props.direction])
const ariaLabel = computed(() => LABEL[props.direction])

/* Le bouton interne garde une épaisseur fixe de 72px sur l'axe du scroll. */
const isHorizontal = computed(() => props.direction === 'left' || props.direction === 'right')
const buttonStyle = computed(() => (isHorizontal.value ? { height: '72px' } : { width: '72px' }))

const classes = computed(() => [
  'button-slider',
  `button-slider--${props.direction}`,
  !props.canScroll && 'button-slider--hidden',
])
</script>

<template>
  <div :class="classes">
    <div class="button-slider__gradient" aria-hidden="true" />
    <buttonMain
      type="primary-on-picture"
      size="small"
      :icon-left="icon"
      :aria-label="ariaLabel"
      :disabled="disabled"
      :style="buttonStyle"
      @click="emit('click', $event)"
    />
  </div>
</template>

<style scoped>
.button-slider {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  opacity: 1;
  visibility: visible;
  transition: opacity 150ms ease;
}

/* Horizontal (left/right) : marge latérale autour du bouton. */
.button-slider--left,
.button-slider--right {
  padding-inline: var(--spacing-extra-small);
}

/* Vertical (top/bottom) : marge haut/bas autour du bouton. */
.button-slider--top,
.button-slider--bottom {
  padding-block: var(--spacing-extra-small);
}

.button-slider--hidden {
  opacity: 0;
  visibility: hidden;
  pointer-events: none;
  transition:
    opacity 150ms ease,
    visibility 0s linear 150ms;
}

.button-slider__gradient {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

/* Le dégradé part du bord de la direction vers l'intérieur. */
.button-slider--left .button-slider__gradient {
  background: linear-gradient(to right, var(--surface-background), transparent);
}

.button-slider--right .button-slider__gradient {
  background: linear-gradient(to left, var(--surface-background), transparent);
}

.button-slider--top .button-slider__gradient {
  background: linear-gradient(to bottom, var(--surface-background), transparent);
}

.button-slider--bottom .button-slider__gradient {
  background: linear-gradient(to top, var(--surface-background), transparent);
}
</style>
