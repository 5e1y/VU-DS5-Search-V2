<script setup lang="ts">
import { computed } from 'vue'

/* Tuile produit horizontale, mises en page compactes (ex. colonne "Produits"
   de la recherche). Pour l'instant seule la taille extraSmall est implémentée. */
type CardSize = 'extraSmall'

const props = withDefaults(
  defineProps<{
    title?: string
    image?: string
    price?: string
    size?: CardSize
    href?: string
    disabled?: boolean
  }>(),
  {
    title: '',
    image: '',
    price: '',
    size: 'extraSmall',
    disabled: false,
  },
)

const tag = computed(() => (props.href ? 'a' : 'button'))

const rootClasses = computed(() => [
  'cpbh',
  `cpbh--${props.size}`,
  props.disabled && 'cpbh--disabled',
])
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
    <span class="cpbh__media">
      <img v-if="image" class="cpbh__img" :src="image" alt="" />
    </span>

    <span class="cpbh__info">
      <span class="cpbh__title medium-200">{{ title }}</span>
      <span class="cpbh__price semi-bold-200">{{ price }}</span>
    </span>
  </component>
</template>

<style scoped>
.cpbh {
  display: flex;
  gap: var(--spacing-small);
  align-items: flex-start;
  width: 100%;
  padding: var(--spacing-extra-small);
  border: var(--stroke-passive) solid var(--interactive-product-box-stroke-default);
  border-radius: var(--radius-large);
  background: var(--surface-background);
  cursor: pointer;
  text-align: left;
  text-decoration: none;
  overflow: clip;
  transition:
    border-color 150ms ease,
    box-shadow 150ms ease;
}

/* Rollover : bordure rollover au survol. */
.cpbh:hover {
  border-color: var(--interactive-product-box-stroke-rollover);
}

/* Focus : bordure rollover + halo focus (spec Figma). */
.cpbh:focus-visible {
  outline: none;
  border-color: var(--interactive-product-box-stroke-rollover);
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

.cpbh--disabled,
.cpbh[aria-disabled='true'] {
  cursor: not-allowed;
  pointer-events: none;
  opacity: 0.5;
}

/* ── Image (ratio 4:5) ── */
.cpbh--extraSmall .cpbh__media {
  flex-shrink: 0;
  height: 72px;
  aspect-ratio: 40 / 50;
  border-radius: var(--radius-medium);
  overflow: clip;
}
.cpbh__img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* ── Infos ── */
.cpbh__info {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-extra-small);
  flex: 1;
  min-width: 0;
  padding: var(--spacing-minuscule) 0;
}
.cpbh__title {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  color: var(--text-icon-neutral-hard);
}
.cpbh__price {
  color: var(--text-icon-neutral-hard);
}
</style>
