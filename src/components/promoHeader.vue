<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'

const props = withDefaults(
  defineProps<{
    device?: 'desktop' | 'mobile'
    promoCode?: string
    discount?: number
    minAmount?: number
    targetDate: Date
  }>(),
  {
    device: 'desktop',
    promoCode: 'VENTEUNIQUE10',
    discount: 10,
    minAmount: 500,
  },
)

const countdown = ref('')
let timer: ReturnType<typeof setInterval>

function updateCountdown() {
  const diff = props.targetDate.getTime() - Date.now()
  if (diff <= 0) {
    countdown.value = '0j 0h 0m 0s'
    clearInterval(timer)
    return
  }
  const j = Math.floor(diff / 86400000)
  const h = Math.floor((diff % 86400000) / 3600000)
  const m = Math.floor((diff % 3600000) / 60000)
  const s = Math.floor((diff % 60000) / 1000)
  countdown.value = `${j}j ${h}h ${m}m ${s}s`
}

onMounted(() => {
  updateCountdown()
  timer = setInterval(updateCountdown, 1000)
})

onUnmounted(() => clearInterval(timer))
</script>

<template>
  <div :class="['promo-header', `promo-header--${device}`]">
    <div class="promo-header__content">
      <span class="normal-200">-{{ discount }}% dès {{ minAmount }}€ avec le code</span>
      <span class="semi-bold-200">{{ promoCode }}</span>
      <span class="normal-200">sur les produits Vente-unique</span>
      <span class="normal-200">se termine dans :</span>
      <span class="semi-bold-200">{{ countdown }}</span>
    </div>
  </div>
</template>

<style scoped>
.promo-header {
  background-color: #000000;
  color: var(--text-icon-neutral-on-dark);
  padding-block: var(--spacing-extra-extra-small);
  width: 100%;
}

.promo-header--desktop {
  padding-inline: 48px;
}

.promo-header--mobile {
  padding-inline: 12px;
}

.promo-header__content {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  justify-content: center;
  gap: 0 var(--spacing-extra-small);
  text-align: center;
}
</style>
