<script setup lang="ts">
import iconMain from './iconMain.vue'

type CaptionType = 'neutral' | 'info' | 'negative' | 'positive' | 'attention'

withDefaults(
  defineProps<{
    type?: CaptionType
    showIcon?: boolean
    icon?: string
    text?: string
  }>(),
  {
    type: 'neutral',
    showIcon: true,
  },
)
</script>

<template>
  <div class="caption-main" :class="`caption-main--${type}`">
    <iconMain v-if="showIcon && icon" :icon="icon" size="medium" class="caption-main__icon" />
    <div class="caption-main__content">
      <slot>
        <p class="medium-200">{{ text }}</p>
      </slot>
    </div>
  </div>
</template>

<style scoped>
.caption-main {
  display: flex;
  gap: var(--spacing-medium);
  padding: var(--spacing-medium);
  border-radius: var(--radius-medium);
  width: 100%;
}

.caption-main__icon {
  flex-shrink: 0;
  align-self: flex-start;
}

.caption-main__content {
  flex: 1;
  min-width: 0;
}

/* ── Type colors ─────────────────────────────────────────── */
.caption-main--neutral {
  background-color: var(--surface-neutral);
  color: var(--text-icon-neutral-medium);
}

.caption-main--info {
  background-color: var(--surface-information);
  color: var(--text-icon-information-medium);
}

.caption-main--negative {
  background-color: var(--surface-negative);
  color: var(--text-icon-negative-medium);
}

.caption-main--positive {
  background-color: var(--surface-positive);
  color: var(--text-icon-positive-medium);
}

.caption-main--attention {
  background-color: var(--surface-attention);
  color: var(--text-icon-attention-medium);
}
</style>
