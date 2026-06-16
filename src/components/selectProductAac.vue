<script setup lang="ts">
import iconMain from './iconMain.vue'
import labelMain from './labelMain.vue'

const props = withDefaults(
  defineProps<{
    title: string
    value?: string
    count?: number
    image?: string
    disabled?: boolean
  }>(),
  {
    value: '',
    disabled: false,
  },
)

const emit = defineEmits<{
  click: []
}>()
</script>

<template>
  <button
    type="button"
    class="select-product-aac"
    :class="{ 'select-product-aac--disabled': disabled }"
    :disabled="disabled"
    :aria-disabled="disabled"
    @click="!disabled && emit('click')"
  >
    <span v-if="image" class="select-product-aac__thumb">
      <img class="select-product-aac__img" :src="image" alt="" />
    </span>

    <span class="select-product-aac__content">
      <span class="select-product-aac__title medium-200">{{ title }}</span>
      <span class="select-product-aac__value normal-200">{{ value }}</span>
      <labelMain
        v-if="count != null"
        class="select-product-aac__count"
        type="neutral"
        size="small"
        :text="String(count)"
      />
    </span>

    <iconMain class="select-product-aac__chevron" icon="ChevronRight" size="large" />
  </button>
</template>

<style scoped>
.select-product-aac {
  display: flex;
  align-items: center;
  width: 100%;
  padding: var(--spacing-extra-small);
  background: var(--surface-background);
  border: none;
  border-radius: var(--radius-medium);
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-default);
  text-align: left;
  cursor: pointer;
  overflow: hidden;
  transition: box-shadow 150ms ease;
}

.select-product-aac:hover {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-rollover);
}

.select-product-aac:focus-visible {
  outline: none;
  box-shadow:
    inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-rollover),
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

/* ── Thumbnail ───────────────────────────────────────────── */
.select-product-aac__thumb {
  flex-shrink: 0;
  width: var(--spacing-extra-extra-large);
  border-radius: var(--radius-small);
  overflow: hidden;
}

.select-product-aac__img {
  display: block;
  width: 100%;
  aspect-ratio: 4 / 3;
  object-fit: cover;
}

/* ── Content ─────────────────────────────────────────────── */
.select-product-aac__content {
  display: flex;
  align-items: center;
  flex: 1;
  min-width: 0;
  gap: var(--spacing-extra-extra-small);
  padding: var(--spacing-extra-extra-small) var(--spacing-extra-small);
}

.select-product-aac__title {
  flex-shrink: 0;
  color: var(--text-icon-neutral-medium);
  white-space: nowrap;
}

.select-product-aac__value {
  flex: 1;
  min-width: 0;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  color: var(--text-icon-neutral-light);
}

.select-product-aac__count {
  flex-shrink: 0;
}

/* ── Chevron ─────────────────────────────────────────────── */
.select-product-aac__chevron {
  flex-shrink: 0;
  color: var(--text-icon-neutral-medium);
  transition: color 150ms ease;
}

.select-product-aac:hover .select-product-aac__chevron,
.select-product-aac:focus-visible .select-product-aac__chevron {
  color: var(--interactive-text-icon-background-rollover);
}

/* ── Disabled ────────────────────────────────────────────── */
.select-product-aac--disabled {
  opacity: 0.5;
  pointer-events: none;
  cursor: default;
}
</style>
