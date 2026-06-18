<script setup lang="ts">
import { computed } from 'vue'

const props = withDefaults(
  defineProps<{
    title?: string
    description?: string
    count?: number
    countLabel?: string
    image?: string
    imageAlt?: string
    href?: string
    disabled?: boolean
  }>(),
  {
    title: '',
    description: '',
    countLabel: 'produits',
    image: '',
    imageAlt: '',
    disabled: false,
  },
)

const tag = computed(() => (props.href ? 'a' : 'button'))

const rootClasses = computed(() => ['bt-large', props.disabled && 'bt-large--disabled'])
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
    <span class="bt-large__image">
      <img v-if="image" class="bt-large__img" :src="image" :alt="imageAlt" />
    </span>

    <span class="bt-large__content">
      <span v-if="title" class="bt-large__title medium-200">{{ title }}</span>
      <span v-if="description" class="bt-large__description normal-100">{{ description }}</span>
      <span v-if="count != null" class="bt-large__count normal-100">{{ count }} {{ countLabel }}</span>
    </span>
  </component>
</template>

<style scoped>
/* ── Base ────────────────────────────────────────────────── */
.bt-large {
  display: flex;
  align-items: stretch;
  gap: var(--spacing-extra-small);
  width: 100%;
  padding: var(--spacing-extra-small);
  border: none;
  border-radius: var(--radius-large);
  background: var(--interactive-tertiary-background-default);
  cursor: pointer;
  text-align: left;
  text-decoration: none;
  overflow: hidden;
  transition:
    background-color 150ms ease,
    box-shadow 150ms ease;
}

.bt-large:hover,
.bt-large:focus-visible {
  background: var(--interactive-tertiary-background-rollover);
}

.bt-large:focus-visible {
  outline: none;
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

.bt-large--disabled,
.bt-large[aria-disabled='true'] {
  cursor: not-allowed;
  pointer-events: none;
  opacity: 0.5;
}

/* ── Image (vignette ambiance) ───────────────────────────── */
.bt-large__image {
  flex-shrink: 0;
  width: 96px;
  aspect-ratio: 96 / 120;
  border-radius: var(--radius-medium);
  background: var(--surface-background);
  overflow: hidden;
}

.bt-large__img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

/* ── Contenu ─────────────────────────────────────────────── */
.bt-large__content {
  flex: 1 0 0;
  min-width: 0;
  display: flex;
  flex-direction: column;
  gap: var(--spacing-extra-extra-small);
  padding: var(--spacing-extra-extra-small);
}

.bt-large__title,
.bt-large__description {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  color: var(--interactive-text-icon-background-default);
  transition: color 150ms ease;
}

.bt-large:hover .bt-large__title,
.bt-large:hover .bt-large__description,
.bt-large:focus-visible .bt-large__title,
.bt-large:focus-visible .bt-large__description {
  color: var(--interactive-text-icon-background-rollover);
}

.bt-large__count {
  margin-top: auto;
  white-space: nowrap;
  color: var(--text-icon-neutral-light);
}
</style>
