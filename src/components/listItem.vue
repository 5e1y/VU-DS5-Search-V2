<script setup lang="ts">
import { computed } from 'vue'
import iconMain from './iconMain.vue'

type ListItemType = 'large' | 'compact'
type ListItemLevel = 'level-0' | 'level-1' | 'level-2'

const props = withDefaults(
  defineProps<{
    text: string
    type?: ListItemType
    level?: ListItemLevel
    iconLeft?: string
    iconRight?: string
    active?: boolean
    disabled?: boolean
    href?: string
  }>(),
  {
    type: 'large',
    level: 'level-0',
    active: false,
    disabled: false,
  },
)

const emit = defineEmits<{
  click: []
}>()

const tag = computed(() => (props.href ? 'a' : 'button'))

type IconSize = 'medium' | 'large'
const iconSize = computed<IconSize>(() => (props.type === 'large' ? 'large' : 'medium'))

const textClass = computed(() => {
  if (props.active) {
    return props.type === 'large' ? 'medium-underline-300' : 'medium-underline-200'
  }
  return props.type === 'large' ? 'medium-300' : 'medium-200'
})

const rootClasses = computed(() => [
  'list-item',
  `list-item--${props.type}`,
  `list-item--${props.level}`,
  { 'list-item--active': props.active, 'list-item--disabled': props.disabled },
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
    @click="!disabled && emit('click')"
  >
    <span v-if="iconLeft" class="list-item__icon">
      <iconMain :icon="iconLeft" :size="iconSize" />
    </span>

    <span class="list-item__text">
      <span :class="textClass">{{ text }}</span>
    </span>

    <span v-if="iconRight" class="list-item__icon">
      <iconMain :icon="iconRight" :size="iconSize" />
    </span>
  </component>
</template>

<style scoped>
/* ── Base ────────────────────────────────────────────────── */
.list-item {
  display: flex;
  align-items: center;
  width: 100%;
  border: none;
  background: none;
  text-align: left;
  text-decoration: none;
  cursor: pointer;
  border-radius: var(--radius-focus-mode);
  color: var(--interactive-text-icon-background-default);
  padding-block: var(--spacing-extra-extra-small);
  padding-right: var(--spacing-extra-small);
  padding-left: var(--spacing-extra-small);
  transition:
    background-color 150ms ease,
    color 150ms ease,
    box-shadow 150ms ease;
}

/* ── Type (height via vertical padding) ──────────────────── */
.list-item--compact {
  padding-block: var(--spacing-none);
}

/* ── Menu level (left indentation) ───────────────────────── */
.list-item--level-1 {
  padding-left: var(--spacing-large);
}

.list-item--level-2 {
  padding-left: var(--spacing-double-large);
}

/* ── Inner parts ─────────────────────────────────────────── */
.list-item__icon {
  display: inline-flex;
  align-items: center;
  flex-shrink: 0;
  padding: var(--spacing-extra-small);
}

.list-item__text {
  display: flex;
  align-items: center;
  flex: 1;
  min-width: 0;
  padding: var(--spacing-extra-small);
}

.list-item__text > span {
  flex: 1;
  min-width: 0;
  word-break: break-word;
}

/* ── Rollover ────────────────────────────────────────────── */
.list-item:hover {
  background: var(--interactive-ghost-background-rollover);
  color: var(--interactive-text-icon-background-rollover);
}

/* ── Focus (keyboard only) ───────────────────────────────── */
.list-item:focus-visible {
  outline: none;
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

/* ── Active (selected) ───────────────────────────────────── */
.list-item--active {
  background: var(--interactive-ghost-background-rollover);
  color: var(--interactive-text-icon-background-rollover);
}

/* ── Disabled ────────────────────────────────────────────── */
.list-item--disabled {
  color: var(--text-icon-neutral-light);
  background: none;
  pointer-events: none;
  cursor: default;
}
</style>
