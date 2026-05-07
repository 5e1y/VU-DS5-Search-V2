<script setup lang="ts">
import { computed } from 'vue'
import { Check } from 'lucide-vue-next'
import iconMain from './iconMain.vue'

type BadgeVariant = 'number' | 'check' | 'dot' | 'none'

const props = withDefaults(
  defineProps<{
    icon: string
    badge?: BadgeVariant
    count?: number
  }>(),
  {
    badge: 'none',
    count: 0,
  },
)

const displayCount = computed(() => (props.count > 99 ? '99+' : String(props.count)))
</script>

<template>
  <div class="icon-badge">
    <iconMain :icon="icon" size="large" />

    <div
      v-if="badge === 'number'"
      class="icon-badge__pastille icon-badge__pastille--number semi-bold-100"
    >
      {{ displayCount }}
    </div>

    <div
      v-else-if="badge === 'check'"
      class="icon-badge__pastille icon-badge__pastille--check"
    >
      <Check :size="12" />
    </div>

    <div v-else-if="badge === 'dot'" class="icon-badge__pastille icon-badge__pastille--dot" />
  </div>
</template>

<style scoped>
.icon-badge {
  position: relative;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  padding: var(--spacing-extra-extra-small);
}

.icon-badge__pastille {
  position: absolute;
  background: var(--surface-notification);
  border-radius: var(--radius-round);
}

.icon-badge__pastille--number {
  bottom: 0;
  right: 0;
  min-width: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  padding-inline: var(--spacing-extra-extra-small);
  color: var(--text-icon-neutral-on-dark);
}

.icon-badge__pastille--check {
  bottom: 0;
  right: 0;
  width: 16px;
  height: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--text-icon-neutral-on-dark);
}

.icon-badge__pastille--dot {
  top: 4px;
  right: 4px;
  width: 8px;
  height: 8px;
}
</style>
