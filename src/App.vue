<script setup lang="ts">
import { ref, computed } from 'vue'
import iconMain from './components/iconMain.vue'
import buttonMain from './components/buttonMain.vue'
import toggleEye from './components/toggleEye.vue'
import inputComp from './components/input.vue'
import selectComp from './components/select.vue'
import largeTextAreaComp from './components/largeTextArea.vue'
import inputFieldFull from './components/inputFieldFull.vue'
import passwordCheck from './components/passwordCheck.vue'
import inputSearchBar from './components/inputSearchBar.vue'
import iconBadge from './components/iconBadge.vue'
import buttonHeader from './components/buttonHeader.vue'
import labelInteractiveChip from './components/labelInteractiveChip.vue'
import textLink from './components/textLink.vue'
import scrollArea from './components/scrollArea.vue'
import labelMain from './components/labelMain.vue'
import captionMain from './components/captionMain.vue'
import buttonSpecialMenu from './components/buttonSpecialMenu.vue'
import vuLogo from './components/vuLogo.vue'
import promoHeader from './components/promoHeader.vue'
import toggleLike from './components/toggleLike.vue'
import toggleMain from './components/toggleMain.vue'
import toggleChoice from './components/toggleChoice.vue'
import toggleFilter from './components/toggleFilter.vue'
import toggleColorFilter from './components/toggleColorFilter.vue'
import websiteHeader from './components/websiteHeader.vue'
import selectProductAac from './components/selectProductAac.vue'
import inputSlider from './components/inputSlider.vue'
import inputSliderButton from './components/inputSliderButton.vue'
import buttonTactile from './components/buttonTactile.vue'
import btLarge from './components/btLarge.vue'
import buttonPagination from './components/buttonPagination.vue'
import pagination from './components/pagination.vue'
import buttonSlider from './components/buttonSlider.vue'
import buttonTab from './components/buttonTab.vue'
import selectTab from './components/selectTab.vue'
import tabList from './components/tabList.vue'
import tab from './components/tab.vue'

// websiteHeader
const headerCartCount = ref(2)
const headerAccountLoggedIn = ref(true)
const headerAccountName = ref('Florian')
const headerPromo = ref(true)
const headerShowLang = ref(false)
const headerExtraLinks = ref([{ label: 'Collection Rouge Passion', href: '#' }])

const eyeValue = ref(false)
const inputDefault = ref('')
const inputPassword = ref('')
const inputError = ref('')
const inputDisabled = ref('Valeur désactivée')

// select
const selectValue = ref('')
const selectOpen = ref(false)
const selectErrorOpen = ref(false)
const selectIconOpen = ref(false)
const selectValueOpen = ref(false)
const selectRangeDefaultOpen = ref(false)
const selectRangeValueOpen = ref(false)

// largeTextArea
const textAreaDefault = ref('')
const textAreaError = ref('')
const textAreaDisabled = ref('Texte désactivé')

// inputFieldFull
const fieldValue = ref('')
const fieldError = ref('')
const selectFieldValue = ref('')
const textAreaFieldValue = ref('')
const fieldSelectOpen = ref(false)

// passwordCheck
const passwordValue = ref('')
const passwordBlurred = ref(false)

const PASSWORD_CRITERIA = [/.{10,}/, /[a-z]/, /[A-Z]/, /[0-9]/, /[^a-zA-Z0-9]/]
const isPasswordValid = computed(() => PASSWORD_CRITERIA.every((r) => r.test(passwordValue.value)))
const passwordInputState = computed(() =>
  passwordBlurred.value && !isPasswordValid.value ? 'error' : 'default',
)

// inputSearchBar
const searchValue = ref('')
const lastSearch = ref('')

// toggleLike
const likeWhite = ref(false)
const likeBlack = ref(false)
const likeWishlist = ref(false)

// toggleMain
const mainCheck = ref(false)
const mainRadio = ref(false)
const mainSwitch = ref(false)

// toggleChoice
const choiceCheck = ref(false)
const choiceRadio = ref(false)
const choiceStrokeCheck = ref(false)
const choiceStrokeRadio = ref(false)
const aacClicks = ref(0)

// labelInteractiveChip
const chips = ref(['Canapé 3 places', 'Couleur beige', 'Prix < 500€'])
function removeChip(i: number) {
  chips.value.splice(i, 1)
}

// toggleFilter
const filterDroit = ref(false)
const filterAngle = ref(true)

// toggleColorFilter
const colorBeige = ref(false)
const colorBlue = ref(true)
const colorGradient = ref(false)
const colorImage = ref(false)

// inputSlider
const sliderLength = ref<[number, number]>([220, 330])
const sliderPrice = ref<[number, number]>([150, 800])
const sliderStep = ref<[number, number]>([20, 80])
const sliderDisabled = ref<[number, number]>([30, 70])

// pagination
const pageSmall = ref(2)
const pageLarge = ref(5)
const paginationDevice = ref<'Desktop' | 'Tablet' | 'Mobile'>('Desktop')
function setPaginationDevice(d: 'Desktop' | 'Tablet' | 'Mobile') {
  paginationDevice.value = d
  document.documentElement.dataset.theme = d
}

// buttonSlider
const sliderCanLeft = ref(false)
const sliderCanRight = ref(true)

// tab / tabList
const tabItems = [
  { label: 'Grand angle gauche', value: 'gag' },
  { label: 'Standard', value: 'std' },
  { label: 'Téléobjectif', value: 'tele' },
  { label: 'Macro', value: 'macro' },
  { label: 'Fisheye', value: 'fish' },
  { label: 'Portrait', value: 'portrait' },
  { label: 'Grand angle droit', value: 'gad' },
]
const tabItemsBadge = [
  { label: 'Description', value: 'desc' },
  { label: 'Caractéristiques', value: 'specs' },
  { label: 'Avis', value: 'reviews', badge: 12 },
  { label: 'Livraison', value: 'shipping', disabled: true },
]
const tabActive = ref('gag')
const tabActiveRange = ref('std')
const tabBadgeActive = ref('desc')
const tabContainerActive = ref('gag')
const selectTabOpen = ref(false)
const selectTabRangeOpen = ref(false)
const tabContainerOpen = ref(false)
</script>

<template>
  <websiteHeader
    :promo="headerPromo"
    :cart-count="headerCartCount"
    :account-logged-in="headerAccountLoggedIn"
    :account-name="headerAccountName"
    :show-language-selection="headerShowLang"
    :extra-nav-links="headerExtraLinks"
    @menu-click="() => {}"
  />

  <main style="padding: 2rem; padding-top: 10rem; display: flex; flex-direction: column; gap: 2rem; font-family: Poppins, sans-serif;">

    <section>
      <h2>websiteHeader — contrôles</h2>
      <div style="display: flex; gap: 1rem; flex-wrap: wrap; align-items: center; font-size: 14px; font-family: Poppins, sans-serif;">
        <label style="display: flex; align-items: center; gap: 6px; cursor: pointer;">
          <input type="checkbox" v-model="headerPromo" /> Promo
        </label>
        <label style="display: flex; align-items: center; gap: 6px; cursor: pointer;">
          <input type="checkbox" v-model="headerAccountLoggedIn" /> Compte connecté
        </label>
        <label style="display: flex; align-items: center; gap: 6px; cursor: pointer;">
          <input type="checkbox" v-model="headerShowLang" /> Sélecteur langue
        </label>
        <label style="display: flex; align-items: center; gap: 6px;">
          Panier :
          <input type="number" v-model.number="headerCartCount" min="0" max="99" style="width: 48px; border: 1px solid #ccc; border-radius: 4px; padding: 2px 6px;" />
        </label>
      </div>
      <p style="font-size: 13px; color: #666; margin-top: 8px; font-family: Poppins, sans-serif;">
        Scroller la page pour voir le header se rétracter en version sticky. Redimensionner à ≤991px pour le layout mobile.
      </p>
    </section>

    <section>
      <h2>iconMain — toutes les tailles</h2>
      <div style="display: flex; align-items: center; gap: 1rem; flex-wrap: wrap;">
        <iconMain icon="Star" size="extra-small" />
        <iconMain icon="Star" size="small" />
        <iconMain icon="Star" size="medium" />
        <iconMain icon="Star" size="large" />
        <iconMain icon="Star" size="extra-large" />
        <iconMain icon="Star" size="extra-extra-large" />
      </div>
      <div style="display: flex; align-items: center; gap: 1rem; margin-top: 1rem; flex-wrap: wrap;">
        <iconMain icon="Star" size="medium" :focus="true" />
        <iconMain icon="ArrowRight" size="large" />
        <iconMain icon="Heart" size="large" />
        <iconMain icon="Search" size="large" />
      </div>
    </section>

    <section>
      <h2>buttonMain — tous les types (default)</h2>
      <div style="display: flex; align-items: center; gap: 1rem; flex-wrap: wrap;">
        <buttonMain type="primary"            label="Primary"            icon-left="ArrowRight" />
        <buttonMain type="secondary"          label="Secondary"          icon-left="ArrowRight" />
        <buttonMain type="tertiary"           label="Tertiary"           icon-left="ArrowRight" />
        <buttonMain type="ghost"              label="Ghost"              icon-left="ArrowRight" />
        <buttonMain type="text"               label="Text"               icon-left="ArrowRight" />
      </div>
      <div style="display: flex; align-items: center; gap: 1rem; margin-top: 1rem; flex-wrap: wrap; background: #888; padding: 1rem; border-radius: 8px;">
        <buttonMain type="primary-on-picture" label="On picture"         icon-left="ArrowRight" />
      </div>
    </section>

    <section>
      <h2>buttonMain — taille small</h2>
      <div style="display: flex; align-items: center; gap: 1rem; flex-wrap: wrap;">
        <buttonMain type="primary"   size="small" label="Primary"   icon-left="ArrowRight" />
        <buttonMain type="secondary" size="small" label="Secondary" icon-left="ArrowRight" />
        <buttonMain type="tertiary"  size="small" label="Tertiary"  icon-left="ArrowRight" />
        <buttonMain type="ghost"     size="small" label="Ghost"     icon-left="ArrowRight" />
        <buttonMain type="text"      size="small" label="Text"      icon-left="ArrowRight" />
      </div>
    </section>

    <section>
      <h2>buttonMain — combinaisons d'icônes</h2>
      <div style="display: flex; align-items: center; gap: 1rem; flex-wrap: wrap;">
        <buttonMain type="primary" label="Icône gauche" icon-left="ArrowRight" />
        <buttonMain type="primary" label="Icône droite" icon-right="ChevronDown" />
        <buttonMain type="primary" label="Deux icônes"  icon-left="Star" icon-right="ChevronDown" />
        <buttonMain type="primary" icon-left="Search" />
      </div>
    </section>

    <section>
      <h2>buttonMain — états disabled</h2>
      <div style="display: flex; align-items: center; gap: 1rem; flex-wrap: wrap;">
        <buttonMain type="primary"   label="Primary"   icon-left="ArrowRight" :disabled="true" />
        <buttonMain type="secondary" label="Secondary" icon-left="ArrowRight" :disabled="true" />
        <buttonMain type="tertiary"  label="Tertiary"  icon-left="ArrowRight" :disabled="true" />
        <buttonMain type="text"      label="Text"      icon-left="ArrowRight" :disabled="true" />
      </div>
    </section>

    <section>
      <h2>buttonMain — lien (href)</h2>
      <div style="display: flex; align-items: center; gap: 1rem; flex-wrap: wrap;">
        <buttonMain type="primary" label="Ouvre vuejs.org" href="https://vuejs.org" icon-right="ExternalLink" />
      </div>
    </section>

    <section>
      <h2>toggleEye — états</h2>
      <div style="display: flex; align-items: center; gap: 1rem; flex-wrap: wrap;">
        <toggleEye v-model="eyeValue" />
        <span style="font-size: 13px; color: #666;">visible: {{ eyeValue }}</span>
        <toggleEye :model-value="false" />
        <toggleEye :model-value="true" />
        <toggleEye :model-value="false" :disabled="true" />
      </div>
    </section>

    <section>
      <h2>input — default</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 400px;">
        <inputComp v-model="inputDefault" placeholder="Placeholder..." />
        <span style="font-size: 13px; color: #666;">Valeur : "{{ inputDefault }}"</span>
      </div>
    </section>

    <section>
      <h2>input — password</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 400px;">
        <inputComp v-model="inputPassword" placeholder="Mot de passe..." :password="true" />
      </div>
    </section>

    <section>
      <h2>input — error</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 400px;">
        <inputComp v-model="inputError" placeholder="Champ en erreur..." state="error" />
        <inputComp v-model="inputError" placeholder="Champ en erreur avec password..." state="error" :password="true" />
      </div>
    </section>

    <section>
      <h2>input — disabled</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 400px;">
        <inputComp v-model="inputDisabled" :disabled="true" />
        <inputComp v-model="inputDisabled" :disabled="true" :password="true" />
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>select — type default</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 400px;">
        <selectComp
          v-model="selectValue"
          placeholder="Sélectionner..."
          :open="selectOpen"
          @click="selectOpen = !selectOpen"
        />
        <span style="font-size: 13px; color: #666;">Valeur : "{{ selectValue }}" — open: {{ selectOpen }}</span>
        <selectComp placeholder="Avec icône" :open="selectIconOpen" show-icon icon="Star" @click="selectIconOpen = !selectIconOpen" />
        <selectComp model-value="Grand angle gauche" :open="selectValueOpen" @click="selectValueOpen = !selectValueOpen" />
        <selectComp placeholder="État error" state="error" :open="selectErrorOpen" @click="selectErrorOpen = !selectErrorOpen" />
        <selectComp model-value="Désactivé" :disabled="true" />
      </div>
    </section>

    <section>
      <h2>select — type range</h2>
      <div style="display: flex; align-items: center; gap: 1rem; flex-wrap: wrap; max-width: 600px;">
        <selectComp type="range" placeholder="Default" :open="selectRangeDefaultOpen" style="width: 200px;" @click="selectRangeDefaultOpen = !selectRangeDefaultOpen" />
        <selectComp type="range" placeholder="Open" :open="selectRangeDefaultOpen" style="width: 200px;" @click="selectRangeDefaultOpen = !selectRangeDefaultOpen" />
        <selectComp type="range" model-value="Avec valeur" :open="selectRangeValueOpen" style="width: 200px;" @click="selectRangeValueOpen = !selectRangeValueOpen" />
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>largeTextArea — default</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 600px;">
        <largeTextAreaComp v-model="textAreaDefault" placeholder="Décrivez votre demande..." />
        <span style="font-size: 13px; color: #666;">Valeur : "{{ textAreaDefault }}"</span>
      </div>
    </section>

    <section>
      <h2>largeTextArea — error</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 600px;">
        <largeTextAreaComp v-model="textAreaError" placeholder="Décrivez votre demande..." state="error" />
      </div>
    </section>

    <section>
      <h2>largeTextArea — disabled</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 600px;">
        <largeTextAreaComp v-model="textAreaDisabled" :disabled="true" />
      </div>
    </section>

    <section>
      <h2>largeTextArea — min/max height</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 600px;">
        <largeTextAreaComp placeholder="minHeight 120, maxHeight 300" :min-height="120" :max-height="300" />
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>inputFieldFull — type field, default</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 600px;">
        <inputFieldFull
          v-model="fieldValue"
          placeholder="Lorem ipsum dolor sit amet"
          show-label label="Label"
          show-geo-select
          show-button button-label="Valider" button-icon-left="Search"
          show-infotext infotext="Infotext"
        />
        <span style="font-size: 13px; color: #666;">Valeur : "{{ fieldValue }}"</span>
      </div>
    </section>

    <section>
      <h2>inputFieldFull — type field, error</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 600px;">
        <inputFieldFull
          v-model="fieldError"
          placeholder="Lorem ipsum dolor sit amet"
          state="error"
          show-label label="Label"
          show-geo-select
          show-button button-label="Valider"
          error-alert="Ce champ est requis"
          show-infotext infotext="Infotext"
        />
      </div>
    </section>

    <section>
      <h2>inputFieldFull — type select</h2>
      <div style="display: flex; flex-direction: column; gap: 2rem; max-width: 600px;">
        <inputFieldFull
          v-model="selectFieldValue"
          type="select"
          placeholder="Sélectionner"
          show-label label="Label"
          :select-open="fieldSelectOpen"
          show-button button-label="Valider"
          show-infotext infotext="Infotext"
          @select-click="fieldSelectOpen = !fieldSelectOpen"
        />
        <inputFieldFull
          type="select"
          placeholder="Sélectionner"
          state="error"
          show-label label="Label"
          show-button button-label="Valider"
          error-alert="Sélection requise"
          show-infotext infotext="Infotext"
        />
      </div>
    </section>

    <section>
      <h2>inputFieldFull — type textArea</h2>
      <div style="display: flex; flex-direction: column; gap: 2rem; max-width: 600px;">
        <inputFieldFull
          v-model="textAreaFieldValue"
          type="textArea"
          placeholder="Décrivez votre demande..."
          show-label label="Label"
          show-infotext infotext="Infotext"
        />
        <inputFieldFull
          type="textArea"
          placeholder="Décrivez votre demande..."
          state="error"
          show-label label="Label"
          show-infotext infotext="Infotext"
        />
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>passwordCheck — dynamique</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 400px;">
        <div
          @focusout="passwordBlurred = true"
          @focusin="passwordBlurred = false"
        >
          <inputComp
            v-model="passwordValue"
            placeholder="Mot de passe..."
            :password="true"
            :state="passwordInputState"
          />
        </div>
        <passwordCheck
          :model-value="passwordValue"
          :blurred="passwordBlurred"
        />
        <span style="font-size: 13px; color: #666;">blurred: {{ passwordBlurred }}</span>
      </div>
    </section>

    <section>
      <h2>passwordCheck — états forcés</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 400px;">
        <p style="font-size: 13px; color: #666;">Default (champ vide, non touché)</p>
        <passwordCheck model-value="" :blurred="false" />
        <p style="font-size: 13px; color: #666;">Error (champ vide, blur)</p>
        <passwordCheck model-value="" :blurred="true" />
        <p style="font-size: 13px; color: #666;">Success (tous critères remplis)</p>
        <passwordCheck model-value="ValidPass1!" :blurred="false" />
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>inputSearchBar — default</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 420px;">
        <inputSearchBar
          v-model="searchValue"
          @search="lastSearch = $event"
        />
        <span style="font-size: 13px; color: #666;">
          Valeur : "{{ searchValue }}" — Dernière recherche : "{{ lastSearch }}"
        </span>
      </div>
    </section>

    <section>
      <h2>inputFieldFull — sans options optionnelles</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 600px;">
        <inputFieldFull placeholder="Field simple, sans label ni bouton" />
        <inputFieldFull type="select" placeholder="Select simple" />
        <inputFieldFull type="textArea" placeholder="TextArea simple" />
      </div>
    </section>


    <section>
      <h2>iconBadge</h2>
      <div style="display: flex; gap: 1.5rem; align-items: center; flex-wrap: wrap;">
        <iconBadge icon="ShoppingBag" badge="number" :count="3" />
        <iconBadge icon="ShoppingBag" badge="number" :count="99" />
        <iconBadge icon="ShoppingBag" badge="number" :count="100" />
        <iconBadge icon="User2" badge="check" />
        <iconBadge icon="User2" badge="dot" />
        <iconBadge icon="HelpCircle" badge="none" />
      </div>
    </section>

    <section>
      <h2>buttonHeader</h2>
      <div style="display: flex; gap: 0.5rem; align-items: center; flex-wrap: wrap;">
        <buttonHeader label="Espace Pro" />
        <buttonHeader label="Aide & Contact" icon="HelpCircle" />
        <buttonHeader label="Se connecter" icon="User2" badge="dot" />
        <buttonHeader label="Florian" icon="User2" badge="check" />
        <buttonHeader label="Panier" icon="ShoppingBag" badge="number" :count="3" />
        <buttonHeader label="Désactivé" icon="ShoppingBag" badge="number" :count="1" :disabled="true" />
      </div>
    </section>


    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>labelInteractiveChip — filtres actifs</h2>
      <div style="display: flex; gap: 0.5rem; flex-wrap: wrap; align-items: center;">
        <labelInteractiveChip
          v-for="(chip, i) in chips"
          :key="chip"
          :text="chip"
          @remove="removeChip(i)"
        />
        <span v-if="chips.length === 0" style="font-size: 13px; color: #666;">Tous les chips ont été retirés — rechargez la page pour réinitialiser.</span>
      </div>
    </section>

    <section>
      <h2>textLink — formes et états</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem;">
        <p style="font-size: 14px; font-family: Poppins, sans-serif;">
          Texte avec un
          <textLink text="lien externe" href="https://vuejs.org" />
          intégré dans un paragraphe.
        </p>
        <p style="font-size: 14px; font-family: Poppins, sans-serif;">
          Texte avec un
          <textLink text="lien avec icône" href="https://vuejs.org" :show-icon="true" />
          et chevron.
        </p>
        <div style="display: flex; gap: 1rem; flex-wrap: wrap; align-items: center;">
          <textLink text="Lien externe (href)" href="https://vuejs.org" />
          <textLink text="Action (button)" @click="() => {}" />
        </div>
      </div>
    </section>

    <section>
      <h2>scrollArea — hauteur dynamique</h2>
      <div style="display: flex; gap: 2rem; flex-wrap: wrap; align-items: flex-start;">
        <scrollArea height="200px" style="width: 280px; background: var(--surface-neutral); border-radius: 8px; padding: 8px;">
          <div v-for="i in 20" :key="i" style="padding: 8px; font-size: 14px; font-family: Poppins, sans-serif; border-bottom: 1px solid rgba(0,0,0,0.05);">
            Ligne {{ i }} — contenu scrollable
          </div>
        </scrollArea>
        <scrollArea height="200px" style="width: 280px; background: var(--surface-neutral); border-radius: 8px; padding: 8px;">
          <div style="padding: 8px; font-size: 14px; font-family: Poppins, sans-serif;">
            Contenu court — pas de scrollbar visible
          </div>
        </scrollArea>
      </div>
    </section>


    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>labelMain</h2>
      <div style="display: flex; flex-wrap: wrap; gap: 8px; align-items: center;">
        <labelMain text="Info" type="info" />
        <labelMain text="Positif" type="positive" />
        <labelMain text="Attention" type="attention" />
        <labelMain text="Négatif" type="negative" />
        <labelMain text="Neutre" type="neutral" />
        <labelMain text="Elevated" type="elevated" />
      </div>
      <div style="display: flex; flex-wrap: wrap; gap: 8px; align-items: center; margin-top: 8px;">
        <labelMain text="Info" type="info" :fill="false" />
        <labelMain text="Positif" type="positive" :fill="false" />
        <labelMain text="Attention" type="attention" :fill="false" />
        <labelMain text="Négatif" type="negative" :fill="false" />
        <labelMain text="Neutre" type="neutral" :fill="false" />
      </div>
      <div style="display: flex; flex-wrap: wrap; gap: 8px; align-items: center; margin-top: 8px;">
        <labelMain text="Large" type="info" size="large" icon-left="CircleDashed" icon-right="CircleDashed" />
        <labelMain text="Icône gauche" type="positive" size="large" icon-left="CircleDashed" />
        <labelMain text="Icône droite" type="negative" icon-right="CircleDashed" />
      </div>
    </section>

    <section>
      <h2>captionMain</h2>
      <div style="display: flex; flex-direction: column; gap: 8px; max-width: 480px;">
        <captionMain type="neutral" icon="CircleDashed" text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor." />
        <captionMain type="info" icon="CircleDashed" text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor." />
        <captionMain type="positive" icon="CircleDashed" text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor." />
        <captionMain type="attention" icon="CircleDashed" text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor." />
        <captionMain type="negative" icon="CircleDashed" text="Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor." />
        <captionMain type="info" :show-icon="false" text="Sans icône — Lorem ipsum dolor sit amet." />
        <captionMain type="neutral" icon="Info">
          <p class="medium-200">Contenu riche via <strong>slot</strong>.</p>
          <p class="medium-200">Deuxième ligne du slot.</p>
        </captionMain>
      </div>
    </section>

    <section>
      <h2>buttonSpecialMenu</h2>
      <div style="display: flex; gap: 16px; align-items: flex-start;">
        <buttonSpecialMenu device="desktop" />
        <buttonSpecialMenu device="mobile" />
        <buttonSpecialMenu device="desktop" :disabled="true" />
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>vuLogo — variantes linguistiques</h2>
      <div style="display: flex; gap: 2rem; align-items: center; flex-wrap: wrap;">
        <div style="width: 180px;">
          <vuLogo type="vente" />
        </div>
        <div style="width: 180px;">
          <vuLogo type="kauf" />
        </div>
        <div style="width: 100px;">
          <vuLogo type="vente" />
        </div>
        <div style="width: 100px;">
          <vuLogo type="kauf" />
        </div>
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section style="padding: 0; overflow: hidden;">
      <h2 style="padding: 0 2rem;">promoHeader — desktop</h2>
      <promoHeader
        device="desktop"
        promo-code="VENTEUNIQUE10"
        :discount="10"
        :min-amount="500"
        :target-date="new Date('2025-12-31T23:59:59')"
      />
    </section>

    <section style="padding: 0; overflow: hidden;">
      <h2 style="padding: 0 2rem;">promoHeader — mobile</h2>
      <div style="width: 390px;">
        <promoHeader
          device="mobile"
          promo-code="VENTEUNIQUE10"
          :discount="10"
          :min-amount="500"
          :target-date="new Date('2025-12-31T23:59:59')"
        />
      </div>
    </section>

    <section style="padding: 0; overflow: hidden;">
      <h2 style="padding: 0 2rem;">promoHeader — code et réductions personnalisés</h2>
      <promoHeader
        device="desktop"
        promo-code="SUMMER20"
        :discount="20"
        :min-amount="300"
        :target-date="new Date('2025-08-31T23:59:59')"
      />
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>toggleLike — color white (product box)</h2>
      <div style="display: flex; gap: 1rem; align-items: center; flex-wrap: wrap; background: #888; padding: 1rem; border-radius: 8px;">
        <toggleLike color="white" v-model="likeWhite" />
        <span style="font-size: 13px; color: white;">liked: {{ likeWhite }}</span>
        <toggleLike color="white" :model-value="true" />
        <toggleLike color="white" :disabled="true" />
      </div>
    </section>

    <section>
      <h2>toggleLike — color black (fiche produit)</h2>
      <div style="display: flex; gap: 1rem; align-items: center; flex-wrap: wrap;">
        <toggleLike color="black" v-model="likeBlack" />
        <span style="font-size: 13px; color: #666;">liked: {{ likeBlack }}</span>
        <toggleLike color="black" :model-value="true" />
        <toggleLike color="black" :disabled="true" />
      </div>
    </section>

    <section>
      <h2>toggleLike — inWishlist (page wishlist)</h2>
      <div style="display: flex; gap: 1rem; align-items: center; flex-wrap: wrap; background: #888; padding: 1rem; border-radius: 8px;">
        <toggleLike color="white" :in-wishlist="true" v-model="likeWishlist" />
        <span style="font-size: 13px; color: white;">clicked: {{ likeWishlist }}</span>
        <toggleLike color="white" :in-wishlist="true" :disabled="true" />
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>toggleMain — checkbox</h2>
      <div style="display: flex; gap: 1.5rem; align-items: center; flex-wrap: wrap;">
        <toggleMain type="check" v-model="mainCheck" />
        <span style="font-size: 13px; color: #666;">checked: {{ mainCheck }}</span>
        <toggleMain type="check" :model-value="false" />
        <toggleMain type="check" :model-value="true" />
        <toggleMain type="check" :model-value="false" :disabled="true" />
        <toggleMain type="check" :model-value="true" :disabled="true" />
      </div>
    </section>

    <section>
      <h2>toggleMain — radio</h2>
      <div style="display: flex; gap: 1.5rem; align-items: center; flex-wrap: wrap;">
        <toggleMain type="radio" v-model="mainRadio" />
        <span style="font-size: 13px; color: #666;">checked: {{ mainRadio }}</span>
        <toggleMain type="radio" :model-value="false" />
        <toggleMain type="radio" :model-value="true" />
        <toggleMain type="radio" :model-value="false" :disabled="true" />
        <toggleMain type="radio" :model-value="true" :disabled="true" />
      </div>
    </section>

    <section>
      <h2>toggleMain — switch</h2>
      <div style="display: flex; gap: 1.5rem; align-items: center; flex-wrap: wrap;">
        <toggleMain type="switch" v-model="mainSwitch" />
        <span style="font-size: 13px; color: #666;">on: {{ mainSwitch }}</span>
        <toggleMain type="switch" :model-value="false" />
        <toggleMain type="switch" :model-value="true" />
        <toggleMain type="switch" :model-value="false" :disabled="true" />
        <toggleMain type="switch" :model-value="true" :disabled="true" />
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>toggleChoice — sans stroke (checkbox)</h2>
      <div style="display: flex; flex-direction: column; max-width: 280px; border-left: 1px solid #eee; padding-left: 1rem;">
        <toggleChoice toggle-type="check" label="Livraison express" v-model="choiceCheck" />
        <toggleChoice toggle-type="check" label="Livraison express" description="Sous 24h" v-model="choiceCheck" />
        <toggleChoice toggle-type="check" label="Désactivé" :disabled="true" />
        <span style="font-size: 13px; color: #666; padding: 4px 0;">checked: {{ choiceCheck }}</span>
      </div>
    </section>

    <section>
      <h2>toggleChoice — sans stroke (radio)</h2>
      <div style="display: flex; flex-direction: column; max-width: 280px; border-left: 1px solid #eee; padding-left: 1rem;">
        <toggleChoice toggle-type="radio" label="Moins de 500€" v-model="choiceRadio" />
        <toggleChoice toggle-type="radio" label="De 500€ à 1000€" description="Large sélection" v-model="choiceRadio" />
        <toggleChoice toggle-type="radio" label="Désactivé" :disabled="true" />
        <span style="font-size: 13px; color: #666; padding: 4px 0;">checked: {{ choiceRadio }}</span>
      </div>
    </section>

    <section>
      <h2>toggleChoice — avec stroke (checkbox)</h2>
      <div style="display: flex; flex-direction: column; gap: 4px; max-width: 280px;">
        <toggleChoice toggle-type="check" :stroke="true" label="Droit" v-model="choiceStrokeCheck" />
        <toggleChoice toggle-type="check" :stroke="true" label="Gauche" description="Orienté gauche" v-model="choiceStrokeCheck" />
        <toggleChoice toggle-type="check" :stroke="true" label="Désactivé" :disabled="true" />
        <span style="font-size: 13px; color: #666;">checked: {{ choiceStrokeCheck }}</span>
      </div>
    </section>

    <section>
      <h2>toggleChoice — avec stroke (radio)</h2>
      <div style="display: flex; flex-direction: column; gap: 4px; max-width: 280px;">
        <toggleChoice toggle-type="radio" :stroke="true" label="Droit" v-model="choiceStrokeRadio" />
        <toggleChoice toggle-type="radio" :stroke="true" label="Gauche" description="Orienté gauche" v-model="choiceStrokeRadio" />
        <toggleChoice toggle-type="radio" :stroke="true" label="Désactivé" :disabled="true" />
        <span style="font-size: 13px; color: #666;">checked: {{ choiceStrokeRadio }}</span>
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>toggleFilter — overlay / panel de filtres</h2>
      <div style="display: flex; flex-direction: column; max-width: 280px; border-left: 1px solid #eee; padding-left: 1rem;">
        <toggleFilter label="Droit" :count="44" v-model="filterDroit" />
        <toggleFilter label="Angle" :count="312" v-model="filterAngle" />
        <toggleFilter label="Convertible" :count="8" :model-value="true" />
        <toggleFilter label="Sans compteur" :model-value="false" />
        <toggleFilter label="Indisponible" :count="0" :disabled="true" />
        <span style="font-size: 13px; color: #666; padding: 4px 0;">Droit: {{ filterDroit }} · Angle: {{ filterAngle }}</span>
      </div>
    </section>

    <section>
      <h2>toggleColorFilter — nuancier de filtres</h2>
      <div style="display: flex; gap: 8px; flex-wrap: wrap; align-items: flex-start;">
        <toggleColorFilter color="#fbe6d0" label="Beige" :count="52" v-model="colorBeige" />
        <toggleColorFilter color="#2b4a7e" label="Bleu nuit" :count="18" v-model="colorBlue" />
        <toggleColorFilter color="#ffffff" label="Blanc" :count="73" :model-value="false" />
        <toggleColorFilter
          color="linear-gradient(135deg, #e74c3c, #f1c40f, #2ecc71, #3498db)"
          label="Multicolore"
          :count="6"
          v-model="colorGradient"
        />
        <toggleColorFilter
          image="https://picsum.photos/seed/wood/64/64"
          label="Bois"
          :count="29"
          v-model="colorImage"
        />
        <toggleColorFilter color="#7f8c8d" label="Gris" :count="4" :disabled="true" />
      </div>
      <span style="font-size: 13px; color: #666;">Beige: {{ colorBeige }} · Bleu: {{ colorBlue }} · Multi: {{ colorGradient }} · Bois: {{ colorImage }}</span>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>selectProductAac — fiche produit (Aide au Choix)</h2>
      <div style="display: flex; flex-direction: column; gap: 8px; max-width: 460px;">
        <selectProductAac title="Couleur :" value="Bleu nuit" :count="9" @click="aacClicks++" />
        <selectProductAac title="Matière :" value="Velours côtelé très résistant et doux au toucher" :count="4" @click="aacClicks++" />
        <selectProductAac title="Modèle :" value="Canapé d'angle 5 places" :count="4" image="https://picsum.photos/seed/sofa/120/90" @click="aacClicks++" />
        <selectProductAac title="Livraison :" value="Standard" @click="aacClicks++" />
        <selectProductAac title="Dimensions :" value="Indisponible" :count="3" :disabled="true" />
        <span style="font-size: 13px; color: #666;">clics : {{ aacClicks }}</span>
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>inputSliderButton — états</h2>
      <div style="display: flex; gap: 1.5rem; align-items: center; flex-wrap: wrap;">
        <inputSliderButton state="default" />
        <inputSliderButton state="rollover" />
        <inputSliderButton state="default" :disabled="true" />
        <span style="font-size: 13px; color: #666;">default (16px) · rollover (24px) · disabled — survoler pour agrandir</span>
      </div>
    </section>

    <section>
      <h2>inputSlider — plage longueur (panel aide au choix)</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 464px;">
        <inputSlider v-model="sliderLength" label="Longueur (cm)" :min="100" :max="400" />
        <span style="font-size: 13px; color: #666;">Valeur : [{{ sliderLength[0] }}, {{ sliderLength[1] }}]</span>
      </div>
    </section>

    <section>
      <h2>inputSlider — plage prix</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 464px;">
        <inputSlider v-model="sliderPrice" label="Prix (€)" :min="0" :max="1000" />
        <span style="font-size: 13px; color: #666;">Valeur : [{{ sliderPrice[0] }}, {{ sliderPrice[1] }}]</span>
      </div>
    </section>

    <section>
      <h2>inputSlider — avec pas (step 5)</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 464px;">
        <inputSlider v-model="sliderStep" label="Note (pas de 5)" :min="0" :max="100" :step="5" />
        <span style="font-size: 13px; color: #666;">Valeur : [{{ sliderStep[0] }}, {{ sliderStep[1] }}]</span>
      </div>
    </section>

    <section>
      <h2>inputSlider — disabled</h2>
      <div style="display: flex; flex-direction: column; gap: 1rem; max-width: 464px;">
        <inputSlider v-model="sliderDisabled" label="Désactivé" :min="0" :max="100" :disabled="true" />
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>buttonTactile — packshot (catégorie phare)</h2>
      <div style="display: flex; gap: 16px; flex-wrap: wrap; align-items: flex-start;">
        <buttonTactile label="Table basse" image="https://picsum.photos/seed/packshot1/248/248" href="#" />
        <buttonTactile label="Canapé d'angle convertible" image="https://picsum.photos/seed/packshot2/248/248" :active="true" href="#" />
        <buttonTactile label="Fauteuil" image="https://picsum.photos/seed/packshot3/248/248" :with-filter="false" href="#" />
        <buttonTactile label="Indisponible" image="https://picsum.photos/seed/packshot4/248/248" :disabled="true" />
      </div>
      <span style="font-size: 13px; color: #666;">default · active (souligné) · sans voile · disabled — survoler / Tab pour rollover &amp; focus</span>
    </section>

    <section>
      <h2>buttonTactile — ambiance (inspiration)</h2>
      <div style="display: flex; gap: 16px; flex-wrap: wrap; align-items: flex-start;">
        <buttonTactile type="ambiance" label="Salon" image="https://picsum.photos/seed/ambiance1/248/360" href="#" />
        <buttonTactile type="ambiance" label="Chambre cosy" image="https://picsum.photos/seed/ambiance2/248/360" :active="true" href="#" />
        <buttonTactile type="ambiance" label="Cuisine" image="https://picsum.photos/seed/ambiance3/248/360" :disabled="true" />
      </div>
      <span style="font-size: 13px; color: #666;">default · active (souligné) · disabled</span>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>btLarge — renvoi vers un catalogue</h2>
      <div style="display: flex; flex-direction: column; gap: 12px; max-width: 460px;">
        <btLarge
          title="Porte à galandage"
          description="Je suis une description du bouton"
          :count="23450"
          image="https://picsum.photos/seed/galandage/96/120"
          image-alt=""
          href="#"
        />
        <btLarge
          title="Titre très long qui doit être tronqué proprement en ellipsis"
          description="Description elle aussi très longue qui dépasse la largeur disponible du bloc"
          :count="128"
          image="https://picsum.photos/seed/catalog2/96/120"
          href="#"
        />
        <btLarge
          title="Sans vignette ni compteur"
          description="Rendu en bouton (pas de href)"
        />
        <btLarge
          title="Indisponible"
          description="État désactivé"
          :count="0"
          image="https://picsum.photos/seed/catalog3/96/120"
          :disabled="true"
        />
        <span style="font-size: 13px; color: #666;">href (lien) · troncature ellipsis · bouton sans image · disabled — survoler / Tab pour rollover &amp; focus</span>
      </div>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>buttonPagination — états (brique atomique)</h2>
      <div style="display: flex; gap: 0.5rem; align-items: center; flex-wrap: wrap;">
        <buttonPagination :page="1" />
        <buttonPagination :page="2" :active="true" />
        <buttonPagination :page="3" />
        <buttonPagination :page="4" :disabled="true" />
        <buttonPagination :page="5" href="#" />
      </div>
      <span style="font-size: 13px; color: #666;">default · active (page courante) · default · disabled · lien (href) — survoler / Tab pour rollover &amp; focus</span>
    </section>

    <section>
      <h2>pagination — assemblage complet (responsive)</h2>
      <div style="display: flex; gap: 6px; align-items: center; flex-wrap: wrap; font-size: 13px; margin-bottom: 1rem;">
        <span style="color: #666;">Device :</span>
        <buttonMain
          v-for="d in (['Desktop', 'Tablet', 'Mobile'] as const)"
          :key="d"
          size="small"
          :type="paginationDevice === d ? 'primary' : 'tertiary'"
          :label="d"
          @click="setPaginationDevice(d)"
        />
        <span style="color: #666; margin-left: 8px;">numéros max : Desktop 7 · Tablet 5 · Mobile 3</span>
      </div>
      <div style="display: flex; flex-direction: column; gap: 1.5rem;">
        <div>
          <pagination v-model="pageLarge" :total="9" />
          <span style="font-size: 13px; color: #666; display: block; margin-top: 6px;">total 9 — page courante : {{ pageLarge }}</span>
        </div>
        <div>
          <pagination v-model="pageSmall" :total="4" />
          <span style="font-size: 13px; color: #666; display: block; margin-top: 6px;">total 4 (pas d'ellipse) — page courante : {{ pageSmall }}</span>
        </div>
      </div>
      <p style="font-size: 13px; color: #666; margin-top: 8px;">
        Changer le device : les numéros du milieu tombent (7→5→3) et les flèches passent en icône seule.
        (Le sélecteur modifie le <code>data-theme</code> global de la page.)
      </p>
    </section>

    <!-- ─────────────────────────────────────────────────────── -->

    <section>
      <h2>buttonSlider — 4 directions (sur fond visible pour le dégradé)</h2>
      <div style="display: flex; gap: 16px; flex-wrap: wrap; align-items: center; background: #888; padding: 1.5rem; border-radius: 8px;">
        <buttonSlider direction="left" />
        <buttonSlider direction="right" />
        <buttonSlider direction="top" />
        <buttonSlider direction="bottom" />
        <buttonSlider direction="left" :disabled="true" />
      </div>
      <span style="font-size: 13px; color: #666;">left · right · top · bottom · disabled — le dégradé part du bord vers l'intérieur (couleur de surface → transparent)</span>
    </section>

    <section>
      <h2>buttonSlider — mini-carrousel (canScroll)</h2>
      <div style="display: flex; gap: 1rem; align-items: center; flex-wrap: wrap; font-size: 13px; margin-bottom: 1rem;">
        <label style="display: flex; align-items: center; gap: 6px; cursor: pointer;">
          <input type="checkbox" v-model="sliderCanLeft" /> canScroll gauche
        </label>
        <label style="display: flex; align-items: center; gap: 6px; cursor: pointer;">
          <input type="checkbox" v-model="sliderCanRight" /> canScroll droite
        </label>
      </div>
      <div style="position: relative; overflow: hidden; border-radius: 8px;">
        <div style="display: flex; gap: 12px; overflow-x: auto; padding: 12px;">
          <div
            v-for="i in 10"
            :key="i"
            style="flex: 0 0 auto; width: 140px; height: 100px; border-radius: 8px; background: var(--surface-neutral); display: flex; align-items: center; justify-content: center; font-family: Poppins, sans-serif; font-size: 14px; color: #666;"
          >
            Item {{ i }}
          </div>
        </div>
        <div style="position: absolute; top: 0; bottom: 0; left: 0; display: flex; align-items: center; padding-left: 8px;">
          <buttonSlider direction="left" :can-scroll="sliderCanLeft" />
        </div>
        <div style="position: absolute; top: 0; bottom: 0; right: 0; display: flex; align-items: center; padding-right: 8px;">
          <buttonSlider direction="right" :can-scroll="sliderCanRight" />
        </div>
      </div>
      <span style="font-size: 13px; color: #666;">Décocher une direction → la flèche et son dégradé disparaissent en fondu (et la flèche n'est plus focusable).</span>
    </section>

    <section>
      <h2>buttonTab — type default (états)</h2>
      <div style="display: flex; gap: 1rem; align-items: center; flex-wrap: wrap; padding: 12px; border-radius: 20px; background: var(--surface-neutral);">
        <buttonTab label="Défaut" style="width: auto;" />
        <buttonTab label="Actif" :active="true" style="width: auto;" />
        <buttonTab label="Avis" :show-badge="true" :count="12" style="width: auto;" />
        <buttonTab label="Désactivé" :disabled="true" style="width: auto;" />
      </div>
      <span style="font-size: 13px; color: #666;">Survol → fond clair ; actif → fond blanc ; focus clavier (Tab) → double anneau. Badge réservé au type default.</span>
    </section>

    <section>
      <h2>buttonTab — type range (hero section)</h2>
      <div style="display: flex; gap: 2px; align-items: center; flex-wrap: wrap; padding: 12px; border-radius: 20px; background: var(--text-icon-neutral-hard);">
        <buttonTab type="range" label="Grand angle gauche" style="width: auto;" />
        <buttonTab type="range" label="Standard" :active="true" style="width: auto;" />
        <buttonTab type="range" label="Téléobjectif" style="width: auto;" />
      </div>
      <span style="font-size: 13px; color: #666;">Variante plus haute (52px), fonds translucides pensés pour un fond image/sombre.</span>
    </section>

    <section>
      <h2>selectTab — default &amp; range</h2>
      <div style="display: flex; gap: 1rem; align-items: flex-start; flex-wrap: wrap;">
        <div style="width: 320px;">
          <selectTab
            :model-value="tabItems.find(i => i.value === tabActive)?.label"
            :open="selectTabOpen"
            @click="selectTabOpen = !selectTabOpen"
          />
        </div>
        <div style="width: 320px; padding: 12px; border-radius: 14px; background: var(--text-icon-neutral-hard);">
          <selectTab
            type="range"
            :show-icon="true"
            icon="Aperture"
            :model-value="tabItems.find(i => i.value === tabActiveRange)?.label"
            :open="selectTabRangeOpen"
            @click="selectTabRangeOpen = !selectTabRangeOpen"
          />
        </div>
      </div>
      <span style="font-size: 13px; color: #666;">Présentationnel comme select.vue : le chevron pivote selon open, le clic émet just « click ». La liste d'options reste gérée par le parent.</span>
    </section>

    <section>
      <h2>tabList — layout tabs (v-model)</h2>
      <div style="padding: 12px; border-radius: 20px; background: var(--surface-neutral);">
        <tabList :items="tabItems" v-model="tabActive" />
      </div>
      <div style="margin-top: 1rem; padding: 12px; border-radius: 20px; background: var(--surface-neutral);">
        <tabList :items="tabItemsBadge" v-model="tabBadgeActive" />
      </div>
      <span style="font-size: 13px; color: #666;">Onglet actif : « {{ tabActive }} » / « {{ tabBadgeActive }} ». Cliquer met à jour le v-model ; séparateurs verticaux entre les onglets ; un item peut être disabled ou porter un badge.</span>
    </section>

    <section>
      <h2>tab — conteneur responsive (data-theme)</h2>
      <div style="display: flex; gap: 1rem; align-items: center; flex-wrap: wrap; font-size: 13px; margin-bottom: 1rem;">
        <button type="button" @click="setPaginationDevice('Desktop')">data-theme = Desktop</button>
        <button type="button" @click="setPaginationDevice('Tablet')">data-theme = Tablet</button>
        <button type="button" @click="setPaginationDevice('Mobile')">data-theme = Mobile</button>
      </div>
      <tab
        :items="tabItems"
        v-model="tabContainerActive"
        :open="tabContainerOpen"
        @toggle="tabContainerOpen = !tabContainerOpen"
      />
      <span style="font-size: 13px; color: #666;">Passer data-theme à « Mobile » (boutons ci-dessus, réutilisés de pagination) → le conteneur bascule en SelectTab (radius medium). Desktop/Tablet → rangée d'onglets (radius large).</span>
    </section>

  </main>
</template>
