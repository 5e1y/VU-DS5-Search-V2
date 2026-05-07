<script setup lang="ts">
defineProps<{
  text: string
}>()

const emit = defineEmits<{
  remove: []
}>()
</script>

<template>
  <div class="chip">
    <div class="chip__label-zone">
      <div class="chip__label">
        <span class="chip__text normal-200">{{ text }}</span>
      </div>
    </div>
    <div class="chip__remove-zone">
      <button class="chip__remove-btn" type="button" @click="emit('remove')">
        <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
          <path d="M18 6 6 18M6 6l12 12"/>
        </svg>
      </button>
    </div>
  </div>
</template>

<style scoped>
.chip {
  display: inline-flex;
  align-items: center;
}

.chip__label-zone {
  padding: var(--spacing-extra-extra-small) 0;
}

.chip__label {
  display: flex;
  align-items: center;
  background: var(--interactive-tertiary-background-default);
  padding: var(--spacing-extra-extra-small) var(--spacing-extra-extra-small) var(--spacing-extra-extra-small) var(--spacing-extra-small);
  border-radius: var(--radius-small) 0 0 var(--radius-small);
}

.chip__text {
  color: var(--text-icon-neutral-light);
  white-space: nowrap;
}

.chip__remove-zone {
  padding: var(--spacing-extra-extra-small) var(--spacing-extra-extra-small) var(--spacing-extra-extra-small) 0;
}

.chip__remove-zone:focus-within {
  box-shadow: 0 0 0 var(--stroke-active) var(--interactive-secondary-stroke-focus);
  border-radius: var(--radius-focus-mode);
  overflow: hidden;
}

.chip__remove-btn {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: var(--spacing-extra-extra-small);
  background: var(--interactive-tertiary-background-default);
  border: none;
  border-left: var(--stroke-passive) solid var(--stroke-neutral-light);
  border-radius: 0 var(--radius-small) var(--radius-small) 0;
  cursor: pointer;
  color: var(--text-icon-neutral-light);
  transition: background 150ms ease;
}

.chip__remove-btn:hover {
  background: var(--interactive-tertiary-background-rollover);
}

.chip__remove-btn:focus-visible {
  outline: none;
  background: var(--interactive-tertiary-background-rollover);
}
</style>
