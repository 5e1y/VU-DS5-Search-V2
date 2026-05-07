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
</script>

<template>
  <main style="padding: 2rem; display: flex; flex-direction: column; gap: 2rem; font-family: Poppins, sans-serif;">

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
        <buttonHeader label="Lien" icon="HelpCircle" href="#" />
      </div>
    </section>

  </main>
</template>
