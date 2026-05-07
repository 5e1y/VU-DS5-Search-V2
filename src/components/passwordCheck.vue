<script setup lang="ts">
import { computed } from 'vue'
import iconMain from './iconMain.vue'

const props = withDefaults(
  defineProps<{
    modelValue?: string
    blurred?: boolean
  }>(),
  {
    modelValue: '',
    blurred: false,
  },
)

type CriterionState = 'default' | 'success' | 'error'

const CRITERIA = [
  { label: 'Au moins 10 caractères', regex: /.{10,}/ },
  { label: '1 minuscule', regex: /[a-z]/ },
  { label: '1 majuscule', regex: /[A-Z]/ },
  { label: '1 chiffre', regex: /[0-9]/ },
  { label: '1 caractère spécial', regex: /[^a-zA-Z0-9]/ },
]

function getCriterionState(regex: RegExp): CriterionState {
  if (regex.test(props.modelValue ?? '')) return 'success'
  if (props.blurred) return 'error'
  return 'default'
}

const criteriaStates = computed(() =>
  CRITERIA.map((c) => ({
    label: c.label,
    state: getCriterionState(c.regex),
  })),
)
</script>

<template>
  <div class="password-check">
    <div
      v-for="criterion in criteriaStates"
      :key="criterion.label"
      class="password-check__item"
      :class="`password-check__item--${criterion.state}`"
    >
      <iconMain
        :icon="criterion.state === 'success' ? 'CheckCircle' : 'X'"
        size="small"
      />
      <span class="password-check__label normal-100">{{ criterion.label }}</span>
    </div>
  </div>
</template>

<style scoped>
/* ── Layout ──────────────────────────────────────────────── */
.password-check {
  display: flex;
  flex-wrap: wrap;
  gap: var(--spacing-extra-extra-small);
  align-items: flex-start;
  align-content: flex-start;
}

.password-check__item {
  display: inline-flex;
  align-items: center;
  gap: var(--spacing-extra-extra-small);
  transition: color 150ms ease;
}

/* ── States ──────────────────────────────────────────────── */
.password-check__item--default {
  color: var(--text-icon-neutral-light);
}

.password-check__item--success {
  color: var(--text-icon-positive-light);
}

.password-check__item--error {
  color: var(--text-icon-negative-light);
}
</style>
