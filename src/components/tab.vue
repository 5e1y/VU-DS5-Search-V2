<script setup lang="ts">
import { computed, onMounted, onUnmounted, ref } from 'vue'
import tabList from './tabList.vue'

interface TabItem {
  label: string
  value?: string | number
  badge?: number
  disabled?: boolean
}

withDefaults(
  defineProps<{
    items?: TabItem[]
    modelValue?: string | number
    type?: 'default' | 'range'
    open?: boolean
  }>(),
  {
    items: () => [],
    type: 'default',
    open: false,
  },
)

const emit = defineEmits<{
  'update:modelValue': [value: string | number]
  toggle: []
}>()

/** Thème courant lu sur la racine (data-theme), observé en réactif. */
const theme = ref('Desktop')
let observer: MutationObserver | null = null

function readTheme() {
  theme.value = document.documentElement.dataset.theme || 'Desktop'
}

onMounted(() => {
  readTheme()
  observer = new MutationObserver(readTheme)
  observer.observe(document.documentElement, {
    attributes: true,
    attributeFilter: ['data-theme'],
  })
})

onUnmounted(() => {
  observer?.disconnect()
})

const isMobile = computed(() => theme.value === 'Mobile')
const layout = computed(() => (isMobile.value ? 'select' : 'tabs'))
</script>

<template>
  <div class="tab" :class="{ 'tab--mobile': isMobile }">
    <tabList
      class="tab__list"
      :items="items"
      :model-value="modelValue"
      :layout="layout"
      :type="type"
      :open="open"
      @update:model-value="emit('update:modelValue', $event)"
      @toggle="emit('toggle')"
    />
  </div>
</template>

<style scoped>
.tab {
  display: flex;
  align-items: center;
  width: 100%;
  padding: var(--spacing-extra-small);
  border-radius: var(--radius-large);
  background: var(--surface-neutral);
}

.tab--mobile {
  border-radius: var(--radius-medium);
}

.tab__list {
  flex: 1;
  min-width: 0;
}
</style>
