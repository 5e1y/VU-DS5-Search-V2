<script setup lang="ts">
import { computed } from 'vue'
import buttonTab from './buttonTab.vue'
import selectTab from './selectTab.vue'

interface TabItem {
  label: string
  value?: string | number
  badge?: number
  disabled?: boolean
}

const props = withDefaults(
  defineProps<{
    items?: TabItem[]
    modelValue?: string | number
    layout?: 'tabs' | 'select'
    type?: 'default' | 'range'
    open?: boolean
  }>(),
  {
    items: () => [],
    layout: 'tabs',
    type: 'default',
    open: false,
  },
)

const emit = defineEmits<{
  'update:modelValue': [value: string | number]
  toggle: []
}>()

/** Valeur d'un item : sa value si fournie, sinon son index. */
function itemValue(item: TabItem, index: number): string | number {
  return item.value ?? index
}

/** Label de l'item actif (pour le layout select). */
const activeLabel = computed(() => {
  const active = props.items.find(
    (item, i) => itemValue(item, i) === props.modelValue,
  )
  return active?.label ?? ''
})
</script>

<template>
  <div class="tab-list" :class="`tab-list--${layout}`">
    <template v-if="layout === 'tabs'">
      <template v-for="(item, index) in items" :key="itemValue(item, index)">
        <span
          v-if="index > 0"
          class="tab-list__separator"
          aria-hidden="true"
        />
        <buttonTab
          class="tab-list__tab"
          :label="item.label"
          :type="type"
          :active="itemValue(item, index) === modelValue"
          :show-badge="item.badge != null"
          :count="item.badge"
          :disabled="item.disabled"
          @click="emit('update:modelValue', itemValue(item, index))"
        />
      </template>
    </template>

    <selectTab
      v-else
      class="tab-list__select"
      :model-value="activeLabel"
      :type="type"
      :open="open"
      @click="emit('toggle')"
    />
  </div>
</template>

<style scoped>
.tab-list {
  display: flex;
  align-items: center;
  gap: 0;
  width: 100%;
}

.tab-list__tab,
.tab-list__select {
  flex: 1;
  min-width: 0;
}

.tab-list__separator {
  flex-shrink: 0;
  width: 1px;
  height: 24px;
  background: var(--surface-separator);
}
</style>
