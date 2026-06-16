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
import websiteHeader from './components/websiteHeader.vue'
import selectProductAac from './components/selectProductAac.vue'
import inputSlider from './components/inputSlider.vue'
import inputSliderButton from './components/inputSliderButton.vue'

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

// inputSlider
const sliderLength = ref<[number, number]>([220, 330])
const sliderPrice = ref<[number, number]>([150, 800])
const sliderStep = ref<[number, number]>([20, 80])
const sliderDisabled = ref<[number, number]>([30, 70])
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

  </main>
</template>
