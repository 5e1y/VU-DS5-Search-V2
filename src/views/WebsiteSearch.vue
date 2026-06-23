<script setup lang="ts">
import { ref, computed } from 'vue'
import inputSearchBar from '../components/inputSearchBar.vue'
import buttonMain from '../components/buttonMain.vue'
import buttonTactile from '../components/buttonTactile.vue'
import sliderRow from '../components/sliderRow.vue'
import toggleLike from '../components/toggleLike.vue'
import searchResult from '../components/searchResult.vue'

type Rect = { top: number; left: number; width: number; height: number }

const props = defineProps<{
  open: boolean
  origin: Rect | null
}>()
const emit = defineEmits<{ close: [] }>()

const query = ref('')

// Suggestions d'autocomplétion (état saisie) : requête en clair + complétion en gras.
const SUFFIXES = [
  '', 'modulable', 'fixe', 'scandinave', 'd’angle', 'cuir',
  'de designers', 'BZ', 'convertible', '2 places', 'gris', 'en velours',
]
const suggestions = computed(() => {
  const q = query.value.trim()
  if (!q) return []
  const list = SUFFIXES.map((post) => ({ pre: '', q, post }))
  list.splice(4, 0, { pre: 'Promotions', q, post: '' })
  return list
})

// Base Vite (ex. "/VU-DS5-Search-V2/" sur GitHub Pages) — pour des assets corrects en sous-chemin.
const base = import.meta.env.BASE_URL
const recentThumb = `${base}figma/prod0.png`

const recent = ref(['Canapé d’angle', 'Canapé d’angle', 'Canapé d’angle', 'Canapé d’angle'])
function removeRecent(i: number) {
  recent.value.splice(i, 1)
}

const categories = [
  'Canapé', 'Meuble de salon', 'Salle à manger', 'Chambre', 'Matelas',
  'Luminaire', 'Mobilier de jardin', 'Meuble salle de bain', 'Verrière', 'Table basse',
].map((label, i) => ({ label, image: `${base}figma/cat${i}.png` }))

const productTitles = [
  'Canapé 2 places en tissu bouclette blanc ivoire OVOTAS',
  'Canapé 3 places en lin et tissu chiné blanc ZULIPRO de Maison Céphy',
]
const products = Array.from({ length: 14 }, (_, i) => ({
  title: productTitles[i % productTitles.length],
  price: '1 149,99 €',
  image: `${base}figma/prod${i}.png`,
  liked: false,
}))

/* ── Morph FLIP : l'overlay est fixe (contraintes Figma), mais à l'ouverture
   le panneau part de la position/taille de la barre de recherche et se déploie
   vers sa position finale. À la fermeture, l'inverse. ── */
function flip(overlayEl: HTMLElement, entering: boolean, done: () => void) {
  const panel = overlayEl.querySelector('.ws-panel') as HTMLElement | null
  const o = props.origin
  if (!panel || !o) {
    overlayEl.style.transition = 'opacity 180ms ease'
    overlayEl.style.opacity = entering ? '0' : '1'
    void overlayEl.offsetHeight
    overlayEl.style.opacity = entering ? '1' : '0'
    setTimeout(done, 180)
    return
  }

  const r = panel.getBoundingClientRect()
  const collapsed =
    `translate(${o.left - r.left}px, ${o.top - r.top}px) ` +
    `scale(${o.width / r.width}, ${o.height / r.height})`

  panel.style.transformOrigin = 'top left'
  overlayEl.style.transition = 'none'
  panel.style.transition = 'none'

  const from = entering ? collapsed : 'none'
  const to = entering ? 'none' : collapsed
  overlayEl.style.opacity = entering ? '0' : '1'
  panel.style.transform = from
  panel.style.opacity = entering ? '0' : '1'

  void overlayEl.offsetHeight // reflow

  overlayEl.style.transition = 'opacity 200ms ease'
  panel.style.transition =
    'transform 280ms cubic-bezier(0.2, 0.8, 0.2, 1), opacity 200ms ease'
  overlayEl.style.opacity = entering ? '1' : '0'
  panel.style.transform = to
  panel.style.opacity = '1'

  const onEnd = (e: TransitionEvent) => {
    if (e.target !== panel || e.propertyName !== 'transform') return
    panel.removeEventListener('transitionend', onEnd)
    panel.style.transform = ''
    panel.style.transition = ''
    done()
  }
  panel.addEventListener('transitionend', onEnd)
}

function onEnter(el: Element, done: () => void) {
  flip(el as HTMLElement, true, done)
}
function onLeave(el: Element, done: () => void) {
  flip(el as HTMLElement, false, done)
}
</script>

<template>
  <Teleport to="body">
    <Transition :css="false" @enter="onEnter" @leave="onLeave">
      <div v-if="open" class="ws-overlay" @click.self="emit('close')">
        <div class="ws-panel shadow-large">
          <!-- Searchbar + Fermer (fixe) -->
          <div class="ws-searchrow">
            <inputSearchBar v-model="query" show-camera class="ws-searchrow__input" />
            <buttonMain type="ghost" label="Fermer" @click="emit('close')" />
          </div>

          <!-- État saisie : suggestions d'autocomplétion -->
          <div v-if="suggestions.length" class="ws-scroll ws-scroll--list">
            <searchResult v-for="(s, i) in suggestions" :key="i" type="present">
              <span v-if="s.pre" class="semi-bold-300">{{ s.pre + ' ' }}</span><span>{{ s.q }}</span><span v-if="s.post" class="semi-bold-300">{{ ' ' + s.post }}</span>
            </searchResult>
          </div>

          <!-- État par défaut : récentes / catégories / produits -->
          <div v-else class="ws-scroll">
            <section class="ws-section">
              <h2 class="ws-section__title medium-400">Recherches récentes</h2>
              <div class="ws-recent">
                <searchResult
                  v-for="(r, i) in recent"
                  :key="i"
                  type="past"
                  :text="r"
                  :image="recentThumb"
                  @clear="removeRecent(i)"
                />
              </div>
            </section>

            <section class="ws-section">
              <h2 class="ws-section__title medium-400">Nos catégories</h2>
              <sliderRow>
                <buttonTactile
                  v-for="(c, i) in categories"
                  :key="i"
                  type="ambiance"
                  :label="c.label"
                  :image="c.image"
                  class="ws-cat"
                />
              </sliderRow>
            </section>

            <section class="ws-section">
              <h2 class="ws-section__title medium-400">Derniers produits consultés</h2>
              <sliderRow>
                <article v-for="(p, i) in products" :key="i" class="ws-card">
                  <div class="ws-card__media">
                    <img :src="p.image" alt="" />
                    <toggleLike v-model="p.liked" class="ws-card__like" />
                  </div>
                  <div class="ws-card__info">
                    <p class="ws-card__title medium-200">{{ p.title }}</p>
                    <p class="ws-card__price semi-bold-200">{{ p.price }}</p>
                  </div>
                </article>
              </sliderRow>
            </section>
          </div>
        </div>
      </div>
    </Transition>
  </Teleport>
</template>

<style scoped>
/* Overlay fixe, contraintes Figma — indépendant de la barre une fois ouvert. */
.ws-overlay {
  position: fixed;
  inset: 0;
  z-index: 1000;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  padding: var(--spacing-large) 48px var(--spacing-huge);
  background: rgba(25, 25, 25, 0.5);
  overflow-y: auto;
}

.ws-panel {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-large);
  width: 994px;
  max-width: 100%;
  max-height: calc(100vh - var(--spacing-large) - var(--spacing-huge));
  padding: var(--spacing-medium);
  background: var(--surface-background);
  border-radius: var(--radius-large);
  overflow: clip;
}

/* Mobile : modale plein écran (pas de marges, pas de coins arrondis). */
@media (max-width: 991px) {
  .ws-overlay {
    padding: 0;
  }
  .ws-panel {
    width: 100%;
    height: 100%;
    max-width: 100%;
    max-height: 100%;
    border-radius: 0;
  }
}

.ws-searchrow {
  display: flex;
  gap: var(--spacing-large);
  align-items: center;
  width: 100%;
  flex-shrink: 0;
}
.ws-searchrow__input {
  flex: 1;
  min-width: 0;
}

/* Zone défilable */
.ws-scroll {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-large);
  flex: 1;
  min-height: 0;
  overflow-y: auto;
  scrollbar-width: thin;
}

/* Liste de suggestions : lignes de 48px collées (gap 0), conforme Figma. */
.ws-scroll--list {
  gap: 0;
}

/* ── Sections ── */
.ws-section {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-small);
  width: 100%;
}
.ws-section__title {
  color: var(--text-icon-neutral-hard);
}

/* ── Recherches récentes (grille responsive) ── */
.ws-recent {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(240px, 1fr));
  gap: var(--spacing-extra-small);
}

.ws-cat {
  flex-shrink: 0;
  width: 144px;
}

/* ── Carte produit ── */
.ws-card {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-extra-small);
  flex-shrink: 0;
  width: 144px;
  padding: var(--spacing-extra-small);
  background: var(--surface-background);
  border: 1px solid var(--interactive-product-box-stroke-default);
  border-radius: var(--radius-large);
  overflow: clip;
}
.ws-card__media {
  position: relative;
  width: 100%;
  aspect-ratio: 40 / 50;
  overflow: clip;
  border-radius: var(--radius-medium);
}
.ws-card__media img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.ws-card__like {
  position: absolute;
  top: 0;
  right: 0;
}
.ws-card__info {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-extra-small);
  height: 88px;
  padding: var(--spacing-extra-small) var(--spacing-extra-extra-small);
}
.ws-card__title {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  color: var(--text-icon-neutral-hard);
}
.ws-card__price {
  color: var(--text-icon-neutral-hard);
}
</style>
