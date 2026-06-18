<script setup lang="ts">
import { computed } from 'vue'

type ButtonTactileType = 'packshot' | 'ambiance'

const props = withDefaults(
  defineProps<{
    label?: string
    image?: string
    type?: ButtonTactileType
    active?: boolean
    withFilter?: boolean
    href?: string
    disabled?: boolean
  }>(),
  {
    label: '',
    image: '',
    type: 'packshot',
    active: false,
    withFilter: true,
    disabled: false,
  },
)

const tag = computed(() => (props.href ? 'a' : 'button'))

const rootClasses = computed(() => [
  'button-tactile',
  `button-tactile--${props.type}`,
  props.active && 'button-tactile--active',
  props.disabled && 'button-tactile--disabled',
])

/* État actif = label souligné (classe utilitaire dédiée). */
const labelClass = computed(() => (props.active ? 'medium-underline-100' : 'medium-100'))
</script>

<template>
  <component
    :is="tag"
    :class="rootClasses"
    :type="tag === 'button' ? 'button' : undefined"
    :disabled="tag === 'button' ? disabled : undefined"
    :aria-disabled="tag === 'a' && disabled ? true : undefined"
    :href="tag === 'a' ? href : undefined"
  >
    <!-- Packshot : produit sur fond blanc, label dans un bloc dédié sous l'image. -->
    <template v-if="type === 'packshot'">
      <span class="button-tactile__media">
        <img v-if="image" class="button-tactile__img" :src="image" alt="" />
        <span v-if="withFilter" class="button-tactile__filter" aria-hidden="true" />
      </span>
      <span class="button-tactile__title">
        <span class="button-tactile__label" :class="labelClass">{{ label }}</span>
      </span>
    </template>

    <!-- Ambiance : image lifestyle plein cadre, label blanc sur scrim en bas. -->
    <template v-else>
      <span class="button-tactile__frame">
        <img v-if="image" class="button-tactile__bg" :src="image" alt="" />
        <span class="button-tactile__spacer" aria-hidden="true" />
        <span class="button-tactile__title button-tactile__title--ambiance">
          <span class="button-tactile__label" :class="labelClass">{{ label }}</span>
        </span>
      </span>
    </template>
  </component>
</template>

<style scoped>
/* ── Base ────────────────────────────────────────────────── */
.button-tactile {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: var(--spacing-none);
  width: 140px;
  padding: var(--spacing-extra-small);
  border: none;
  border-radius: var(--radius-large);
  background: var(--surface-neutral);
  cursor: pointer;
  text-decoration: none;
  overflow: hidden;
  transition:
    background-color 150ms ease,
    box-shadow 150ms ease;
}

.button-tactile:hover,
.button-tactile:focus-visible {
  background: var(--interactive-tertiary-background-rollover);
}

.button-tactile:focus-visible {
  outline: none;
  box-shadow: inset 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-focus);
}

.button-tactile--disabled,
.button-tactile[aria-disabled='true'] {
  cursor: not-allowed;
  pointer-events: none;
  opacity: 0.5;
}

/* ── Média (image carrée) ────────────────────────────────── */
.button-tactile__media {
  position: relative;
  width: 100%;
  aspect-ratio: 1 / 1;
  border-radius: var(--radius-medium);
  overflow: hidden;
  background: var(--surface-background);
}

.button-tactile__img {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: contain;
}

/* Voile d'assombrissement (packshot). */
.button-tactile__filter {
  position: absolute;
  inset: 0;
  border-radius: var(--radius-medium);
  background: var(--effect-image-overlay-default);
  pointer-events: none;
  transition: background-color 150ms ease;
}

.button-tactile:hover .button-tactile__filter,
.button-tactile:focus-visible .button-tactile__filter {
  background: var(--effect-image-overlay-rollover);
}

/* ── Titre / label ───────────────────────────────────────── */
.button-tactile__title {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  height: 56px;
  padding: var(--spacing-extra-extra-small);
}

.button-tactile__label {
  width: 100%;
  text-align: center;
  overflow: hidden;
  text-overflow: ellipsis;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  transition: color 150ms ease;
}

/* Packshot : label sombre, plus appuyé au survol / focus / actif. */
.button-tactile--packshot .button-tactile__label {
  color: var(--interactive-text-icon-background-default);
}

.button-tactile--packshot:hover .button-tactile__label,
.button-tactile--packshot:focus-visible .button-tactile__label,
.button-tactile--packshot.button-tactile--active .button-tactile__label {
  color: var(--interactive-text-icon-background-rollover);
}

/* ── Ambiance (image plein cadre + scrim) ────────────────── */
.button-tactile__frame {
  position: relative;
  width: 100%;
  border-radius: var(--radius-medium);
  overflow: hidden;
}

.button-tactile__bg {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.button-tactile__spacer {
  display: block;
  width: 100%;
  aspect-ratio: 1 / 1;
}

.button-tactile__title--ambiance {
  position: relative;
  background: linear-gradient(to bottom, transparent, var(--effect-image-scrim-strong));
}

.button-tactile--ambiance .button-tactile__label {
  color: var(--text-icon-neutral-on-dark);
}
</style>
