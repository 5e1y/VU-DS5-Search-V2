<script setup lang="ts">
import iconMain from './iconMain.vue'

withDefaults(
  defineProps<{
    device?: 'desktop' | 'mobile'
    disabled?: boolean
  }>(),
  {
    device: 'desktop',
    disabled: false,
  },
)

defineEmits<{ click: [] }>()
</script>

<template>
  <button
    class="btn-menu"
    :class="[`btn-menu--${device}`]"
    :disabled="disabled"
    @click="$emit('click')"
  >
    <iconMain icon="Menu" :size="device === 'mobile' ? 'large' : 'medium'" />
    <span v-if="device === 'desktop'" class="btn-menu__label medium-200">
      Tous nos produits
    </span>
    <span v-if="device === 'mobile'" class="btn-menu__label-mobile medium-100">
      Menu
    </span>
  </button>
</template>

<style scoped>
/* ── Base ────────────────────────────────────────────────── */
.btn-menu {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  border: none;
  background: transparent;
  cursor: pointer;
  border-radius: var(--radius-large);
  color: var(--interactive-text-icon-background-default);
  transition:
    background-color 150ms ease,
    color 150ms ease,
    box-shadow 150ms ease;
}

.btn-menu:hover {
  background-color: var(--interactive-ghost-background-rollover);
}

.btn-menu:focus-visible {
  outline: none;
  background-color: var(--interactive-ghost-background-rollover);
  box-shadow:
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-default);
}

.btn-menu:disabled {
  cursor: not-allowed;
  pointer-events: none;
  color: var(--interactive-text-icon-background-disabled);
}

/* ── Desktop variant ─────────────────────────────────────── */
.btn-menu--desktop {
  padding: var(--spacing-small);
}

.btn-menu__label {
  padding-inline: var(--spacing-extra-extra-small);
  white-space: nowrap;
}

/* ── Mobile variant ──────────────────────────────────────── */
.btn-menu--mobile {
  flex-direction: column;
  width: 48px;
  height: 48px;
  padding: 0;
  gap: 0;
}

.btn-menu__label-mobile {
  white-space: nowrap;
}
</style>
