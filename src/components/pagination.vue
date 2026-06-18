<script setup lang="ts">
import { computed, onBeforeUnmount, onMounted, ref } from 'vue'
import buttonMain from './buttonMain.vue'
import buttonPagination from './buttonPagination.vue'

const props = withDefaults(
  defineProps<{
    modelValue?: number
    total: number
  }>(),
  {
    modelValue: 1,
  },
)

const emit = defineEmits<{
  'update:modelValue': [value: number]
}>()

/* ── Device courant (lu sur [data-theme] de <html>, réactif) ─── */
type Device = 'Desktop' | 'Tablet' | 'Mobile'

const device = ref<Device>('Desktop')
let observer: MutationObserver | null = null

function readDevice() {
  const theme = document.documentElement.dataset.theme
  device.value = theme === 'Tablet' || theme === 'Mobile' ? theme : 'Desktop'
}

onMounted(() => {
  readDevice()
  observer = new MutationObserver(readDevice)
  observer.observe(document.documentElement, {
    attributes: true,
    attributeFilter: ['data-theme'],
  })
})

onBeforeUnmount(() => observer?.disconnect())

const showLabel = computed(() => device.value === 'Desktop')

/* Nombre max de numéros visibles selon le device. */
const MAX_NUMBERS: Record<Device, number> = {
  Desktop: 7,
  Tablet: 5,
  Mobile: 3,
}

/* ── Fenêtrage : numéros + ellipses ('…') ──────────────────── */
type PageItem = number | '…'

const items = computed<PageItem[]>(() =>
  visiblePages(props.modelValue, props.total, MAX_NUMBERS[device.value]),
)

function visiblePages(current: number, total: number, maxNumbers: number): PageItem[] {
  if (total <= 0) return []
  if (total <= maxNumbers) return Array.from({ length: total }, (_, i) => i + 1)

  /* On réserve toujours la première (1) et la dernière (total). */
  const inner = maxNumbers - 2
  let start = current - Math.floor((inner - 1) / 2)
  let end = current + Math.ceil((inner - 1) / 2)

  if (start < 2) {
    end += 2 - start
    start = 2
  }
  if (end > total - 1) {
    start -= end - (total - 1)
    end = total - 1
  }
  start = Math.max(2, start)

  const result: PageItem[] = [1]
  if (start > 2) result.push('…')
  for (let p = start; p <= end; p++) result.push(p)
  if (end < total - 1) result.push('…')
  result.push(total)
  return result
}

const isFirst = computed(() => props.modelValue <= 1)
const isLast = computed(() => props.modelValue >= props.total)

function goTo(page: number) {
  if (page < 1 || page > props.total || page === props.modelValue) return
  emit('update:modelValue', page)
}
</script>

<template>
  <nav class="pagination" aria-label="Pagination">
    <buttonMain
      type="tertiary"
      aria-label="Précédent"
      icon-left="ChevronLeft"
      :label="showLabel ? 'Précédent' : undefined"
      :disabled="isFirst"
      @click="goTo(modelValue - 1)"
    />

    <template v-for="(item, index) in items">
      <span
        v-if="item === '…'"
        :key="`ellipsis-${index}`"
        class="pagination__ellipsis medium-300"
        aria-hidden="true"
      >
        …
      </span>
      <buttonPagination
        v-else
        :key="`page-${item}`"
        :page="item"
        :active="item === modelValue"
        @click="goTo(item)"
      />
    </template>

    <buttonMain
      type="tertiary"
      aria-label="Suivant"
      icon-right="ChevronRight"
      :label="showLabel ? 'Suivant' : undefined"
      :disabled="isLast"
      @click="goTo(modelValue + 1)"
    />
  </nav>
</template>

<style scoped>
.pagination {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: var(--spacing-extra-extra-small);
}

.pagination__ellipsis {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: var(--spacing-small);
  color: var(--interactive-text-icon-background-default);
  user-select: none;
}
</style>
