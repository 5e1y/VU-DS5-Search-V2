<script setup lang="ts">
import iconMain from './iconMain.vue'

type LabelType = 'info' | 'positive' | 'attention' | 'negative' | 'neutral' | 'elevated'
type LabelSize = 'small' | 'large'

withDefaults(
  defineProps<{
    text?: string
    type?: LabelType
    fill?: boolean
    size?: LabelSize
    iconLeft?: string
    iconRight?: string
  }>(),
  {
    text: 'Label',
    type: 'info',
    fill: true,
    size: 'small',
  },
)
</script>

<template>
  <div
    class="label-main"
    :class="[
      `label-main--${type}`,
      fill ? 'label-main--fill' : 'label-main--no-fill',
      size === 'large' && 'label-main--large',
      type === 'elevated' && fill && 'shadow-small',
    ]"
  >
    <iconMain v-if="iconLeft" :icon="iconLeft" size="extra-small" />
    <span class="label-main__text" :class="size === 'large' ? 'normal-200' : 'normal-100'">
      {{ text }}
    </span>
    <iconMain v-if="iconRight" :icon="iconRight" size="extra-small" />
  </div>
</template>

<style scoped>
.label-main {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

/* ── Fill variant ────────────────────────────────────────── */
.label-main--fill {
  padding: var(--spacing-extra-extra-small);
  border-radius: var(--radius-small);
  min-width: 24px;
}

.label-main--fill .label-main__text {
  padding-inline: var(--spacing-extra-extra-small);
}

/* ── No-fill variant ─────────────────────────────────────── */
.label-main--no-fill {
  gap: var(--spacing-extra-extra-small);
}

/* ── Type colors — fill ──────────────────────────────────── */
.label-main--info.label-main--fill {
  background-color: var(--surface-information);
  color: var(--text-icon-information-medium);
}

.label-main--positive.label-main--fill {
  background-color: var(--surface-positive);
  color: var(--text-icon-positive-medium);
}

.label-main--attention.label-main--fill {
  background-color: var(--surface-attention);
  color: var(--text-icon-attention-medium);
}

.label-main--negative.label-main--fill {
  background-color: var(--surface-negative);
  color: var(--text-icon-negative-medium);
}

.label-main--neutral.label-main--fill {
  background-color: var(--surface-neutral);
  color: var(--text-icon-neutral-light);
}

.label-main--elevated.label-main--fill {
  background-color: var(--surface-background);
  color: var(--text-icon-neutral-medium);
}

/* ── Type colors — no-fill ───────────────────────────────── */
.label-main--info.label-main--no-fill {
  color: var(--text-icon-information-light);
}

.label-main--positive.label-main--no-fill {
  color: var(--text-icon-positive-light);
}

.label-main--attention.label-main--no-fill {
  color: var(--text-icon-attention-light);
}

.label-main--negative.label-main--no-fill {
  color: var(--text-icon-negative-light);
}

.label-main--neutral.label-main--no-fill {
  color: var(--text-icon-neutral-light);
}
</style>
