<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted } from 'vue'
import VuLogo from './vuLogo.vue'
import PromoHeader from './promoHeader.vue'
import InputSearchBar from './inputSearchBar.vue'
import ButtonHeader from './buttonHeader.vue'
import ButtonSpecialMenu from './buttonSpecialMenu.vue'
import ButtonMain from './buttonMain.vue'
import SelectInput from './select.vue'

interface NavLink {
  label: string
  href?: string
}

const props = withDefaults(
  defineProps<{
    promo?: boolean
    promoCode?: string
    promoDiscount?: number
    promoMinAmount?: number
    promoTargetDate?: Date
    showLanguageSelection?: boolean
    extraNavLinks?: NavLink[]
    cartCount?: number
    accountLoggedIn?: boolean
    accountName?: string
  }>(),
  {
    promo: true,
    promoCode: 'VENTEUNIQUE10',
    promoDiscount: 10,
    promoMinAmount: 500,
    showLanguageSelection: false,
    cartCount: 0,
    accountLoggedIn: false,
    accountName: '',
  },
)

const emit = defineEmits<{
  menuClick: []
  search: [value: string]
}>()

const isDesktop = ref(true)
const isScrolled = ref(false)
const searchValue = ref('')

// Overlay de recherche : on capture la position de la barre au clic
// (point de départ du morph), mais l'overlay s'ouvre en position fixe Figma.
const searchOpen = ref(false)
const searchRect = ref<{ top: number; left: number; width: number; height: number } | null>(null)
function openSearch(e: Event) {
  const el = (e.currentTarget as HTMLElement).getBoundingClientRect()
  searchRect.value = { top: el.top, left: el.left, width: el.width, height: el.height }
  searchOpen.value = true
}
function closeSearch() {
  searchOpen.value = false
}

const promoTargetDateResolved = computed<Date>(() => {
  if (props.promoTargetDate) return props.promoTargetDate
  const d = new Date()
  d.setDate(d.getDate() + 7)
  return d
})

const FIXED_NAV_LINKS: NavLink[] = [
  { label: 'Promos', href: '#' },
  { label: 'Nouveautés', href: '#' },
  { label: 'Inspirations & Conseils', href: '#' },
  { label: 'Corner Prestige', href: '#' },
]

const limitedExtraLinks = computed(() => (props.extraNavLinks ?? []).slice(0, 3))
const allNavLinks = computed(() => [...limitedExtraLinks.value, ...FIXED_NAV_LINKS])

let mql: MediaQueryList | null = null

function onBreakpointChange(e: MediaQueryListEvent) {
  isDesktop.value = e.matches
}

function onScroll() {
  isScrolled.value = window.scrollY > 100
}

onMounted(() => {
  mql = window.matchMedia('(min-width: 992px)')
  isDesktop.value = mql.matches
  mql.addEventListener('change', onBreakpointChange)
  window.addEventListener('scroll', onScroll, { passive: true })
})

onUnmounted(() => {
  mql?.removeEventListener('change', onBreakpointChange)
  window.removeEventListener('scroll', onScroll)
})
</script>

<template>
  <header class="website-header">

    <!-- ══ DESKTOP ══════════════════════════════════════════════ -->
    <template v-if="isDesktop">

      <!-- Promo banner — se rétracte au scroll -->
      <div
        v-if="promo"
        class="wh-promo-wrapper"
        :class="{ 'wh-promo-wrapper--hidden': isScrolled }"
      >
        <PromoHeader
          device="desktop"
          :promoCode="promoCode"
          :discount="promoDiscount"
          :minAmount="promoMinAmount"
          :targetDate="promoTargetDateResolved"
        />
      </div>

      <div class="wh-main wh-main--desktop">

        <!-- Ligne primaire : logo | searchbar | boutons -->
        <div class="wh-primary">
          <div class="wh-logo wh-logo--desktop">
            <VuLogo />
          </div>

          <div class="wh-search">
            <div
              class="wh-search__field"
              @mousedown="openSearch"
              @focusin="openSearch"
            >
              <InputSearchBar
                v-model="searchValue"
                @search="emit('search', $event)"
              />
            </div>
          </div>

          <div class="wh-actions">
            <ButtonHeader class="wh-espace-pro" label="Espace Pro" />
            <ButtonHeader label="Aide & Contact" icon="HelpCircle" />
            <ButtonHeader
              :label="accountLoggedIn ? accountName : 'Se connecter'"
              icon="User2"
              :badge="accountLoggedIn ? 'check' : 'dot'"
            />
            <ButtonHeader
              label="Panier"
              icon="ShoppingBag"
              badge="number"
              :count="cartCount"
            />
          </div>
        </div>

        <!-- Ligne secondaire — se rétracte au scroll -->
        <div
          class="wh-secondary"
          :class="{ 'wh-secondary--collapsed': isScrolled }"
        >
          <div class="wh-nav">
            <ButtonSpecialMenu device="desktop" @click="emit('menuClick')" />
            <div class="wh-separator" aria-hidden="true" />
            <nav class="wh-navlinks" aria-label="Navigation principale">
              <ButtonMain
                v-for="link in allNavLinks"
                :key="link.label"
                type="ghost"
                size="small"
                :label="link.label"
                :href="link.href"
              />
            </nav>
          </div>
          <div v-if="showLanguageSelection" class="wh-lang">
            <SelectInput model-value="FR" />
          </div>
        </div>

      </div>
    </template>

    <!-- ══ MOBILE ════════════════════════════════════════════════ -->
    <template v-else>

      <!-- Promo banner — toujours visible sur mobile -->
      <PromoHeader
        v-if="promo"
        device="mobile"
        :promoCode="promoCode"
        :discount="promoDiscount"
        :minAmount="promoMinAmount"
        :targetDate="promoTargetDateResolved"
      />

      <div class="wh-main wh-main--mobile">

        <!-- Ligne primaire : menu+logo | boutons icône -->
        <div class="wh-primary wh-primary--mobile">
          <div class="wh-left">
            <ButtonSpecialMenu device="mobile" @click="emit('menuClick')" />
            <div class="wh-logo wh-logo--mobile">
              <VuLogo />
            </div>
          </div>
          <div class="wh-actions">
            <ButtonHeader icon="HelpCircle" />
            <ButtonHeader
              icon="User2"
              :badge="accountLoggedIn ? 'check' : 'dot'"
            />
            <ButtonHeader
              icon="ShoppingBag"
              badge="number"
              :count="cartCount"
            />
          </div>
        </div>

        <!-- Ligne recherche -->
        <div class="wh-search-row">
          <div
            class="wh-searchbar-wrapper"
            @mousedown="openSearch"
            @focusin="openSearch"
          >
            <InputSearchBar
              v-model="searchValue"
              @search="emit('search', $event)"
            />
          </div>
          <div v-if="showLanguageSelection" class="wh-lang">
            <SelectInput model-value="FR" />
          </div>
        </div>

      </div>
    </template>

    <slot name="search-overlay" :open="searchOpen" :close="closeSearch" :origin="searchRect" />
  </header>
</template>

<style scoped>
/* ── Root ────────────────────────────────────────────────────── */
.website-header {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1000;
  display: flex;
  flex-direction: column;
  align-items: stretch;
  background: var(--surface-background);
  border-bottom: 1px solid rgba(197, 185, 187, 0.14);
}

/* ── Promo wrapper (desktop) ─────────────────────────────────── */
.wh-promo-wrapper {
  overflow: hidden;
  max-height: 40px;
  opacity: 1;
  transition:
    max-height 0.3s ease,
    opacity 0.25s ease;
}

.wh-promo-wrapper--hidden {
  max-height: 0;
  opacity: 0;
}

/* ── Main content block ──────────────────────────────────────── */
.wh-main {
  display: flex;
  flex-direction: column;
  align-items: stretch;
  width: 100%;
}

.wh-main--desktop {
  padding-inline: 48px;
  padding-block: var(--spacing-extra-small);
}

.wh-main--mobile {
  padding-inline: 12px;
  padding-block: var(--spacing-extra-small);
  gap: var(--spacing-extra-small);
}

/* ── Primary row ─────────────────────────────────────────────── */
.wh-primary {
  display: flex;
  align-items: center;
  width: 100%;
}

.wh-primary--mobile {
  justify-content: space-between;
  height: 48px;
}

/* ── Logo ────────────────────────────────────────────────────── */
.wh-logo--desktop {
  width: 220px;
  flex-shrink: 0;
}

.wh-logo--mobile {
  flex: 1;
  min-width: 0;
  max-width: 170px;
}

/* ── Search (desktop) ────────────────────────────────────────── */
.wh-search {
  flex: 1;
  min-width: 0;
  padding-inline: 24px;
}

/* Point d'ancrage de la barre (départ du morph) ; l'overlay s'ouvre en fixe. */
.wh-search__field {
  position: relative;
}

/* ── Header action buttons ───────────────────────────────────── */
.wh-actions {
  display: flex;
  align-items: center;
  flex-shrink: 0;
}

/* ── Mobile left group ───────────────────────────────────────── */
.wh-left {
  display: flex;
  align-items: center;
  gap: var(--spacing-extra-small);
  flex: 1;
  min-width: 0;
  padding-right: var(--spacing-extra-extra-small);
}

/* ── Secondary row (desktop) ─────────────────────────────────── */
.wh-secondary {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: var(--spacing-extra-extra-small);
  overflow: hidden;
  max-height: 56px;
  margin-top: var(--spacing-extra-small);
  opacity: 1;
  transition:
    max-height 0.3s ease,
    opacity 0.25s ease,
    margin-top 0.3s ease;
}

.wh-secondary--collapsed {
  max-height: 0;
  opacity: 0;
  margin-top: 0;
}

/* ── Nav (desktop) ───────────────────────────────────────────── */
.wh-nav {
  display: flex;
  align-items: center;
  gap: var(--spacing-extra-extra-small);
  flex: 1;
  min-width: 0;
}

.wh-navlinks {
  display: flex;
  align-items: center;
}

/* ── Separator ───────────────────────────────────────────────── */
.wh-separator {
  width: 1px;
  height: 32px;
  flex-shrink: 0;
  background: rgba(197, 185, 187, 0.4);
}

/* ── Espace Pro — masqué sous 1500px ─────────────────────────── */
@media (max-width: 1499px) {
  .wh-espace-pro {
    display: none;
  }
}

/* ── Language selector ───────────────────────────────────────── */
.wh-lang {
  width: 96px;
  flex-shrink: 0;
}

.wh-lang :deep(.select__content),
.wh-lang :deep(.select__text) {
  min-width: fit-content;
}

/* ── Search row (mobile) ─────────────────────────────────────── */
.wh-search-row {
  display: flex;
  align-items: center;
  gap: var(--spacing-extra-extra-small);
}

.wh-searchbar-wrapper {
  flex: 1;
  min-width: 0;
}
</style>
