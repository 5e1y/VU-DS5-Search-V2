<script setup lang="ts">
import { ref, computed, onMounted, onUnmounted, watch } from 'vue'
import iconMain from './iconMain.vue'

const props = withDefaults(
  defineProps<{
    modelValue?: string
    showCamera?: boolean
  }>(),
  {
    modelValue: '',
    showCamera: false,
  },
)

const emit = defineEmits<{
  'update:modelValue': [value: string]
  search: [value: string]
}>()

const inputRef = ref<HTMLInputElement | null>(null)

const filled = computed(() => (props.modelValue ?? '') !== '')

// ── Animated placeholder ──────────────────────────────────────────
const PHRASES = ['Recherche un produit', 'Rechercher un catalogue']
const TYPING_SPEED = 80   // ms per character
const ERASE_SPEED = 40    // ms per character
const PAUSE_FULL = 1500   // ms pause when phrase is fully typed
const PAUSE_EMPTY = 300   // ms pause before starting next phrase

const animatedPlaceholder = ref('')
let timeoutId: ReturnType<typeof setTimeout> | null = null
let phraseIndex = 0
let charIndex = 0
let isDeleting = false

function tick() {
  if (filled.value) {
    timeoutId = setTimeout(tick, 200)
    return
  }

  const currentPhrase = PHRASES[phraseIndex]!

  if (!isDeleting) {
    charIndex++
    animatedPlaceholder.value = currentPhrase.slice(0, charIndex)

    if (charIndex === currentPhrase.length) {
      isDeleting = true
      timeoutId = setTimeout(tick, PAUSE_FULL)
      return
    }
    timeoutId = setTimeout(tick, TYPING_SPEED)
  } else {
    charIndex--
    animatedPlaceholder.value = currentPhrase.slice(0, charIndex)

    if (charIndex === 0) {
      isDeleting = false
      phraseIndex = (phraseIndex + 1) % PHRASES.length
      timeoutId = setTimeout(tick, PAUSE_EMPTY)
      return
    }
    timeoutId = setTimeout(tick, ERASE_SPEED)
  }
}

onMounted(() => {
  timeoutId = setTimeout(tick, PAUSE_EMPTY)
})

onUnmounted(() => {
  if (timeoutId !== null) clearTimeout(timeoutId)
})

// Resume animation when input is cleared
watch(filled, (isFilled) => {
  if (!isFilled && timeoutId === null) {
    timeoutId = setTimeout(tick, PAUSE_EMPTY)
  }
})

// ── Actions ───────────────────────────────────────────────────────
function handleSearch() {
  emit('search', props.modelValue ?? '')
  inputRef.value?.focus()
}

function handleClear() {
  emit('update:modelValue', '')
  inputRef.value?.focus()
}
</script>

<template>
  <div class="input-search-bar">
    <button
      class="input-search-bar__btn"
      type="button"
      aria-label="Rechercher"
      @click="handleSearch"
    >
      <iconMain icon="Search" size="large" />
    </button>

    <input
      ref="inputRef"
      class="input-search-bar__field"
      :class="{ 'input-search-bar__field--filled': filled }"
      type="search"
      :value="modelValue"
      :placeholder="animatedPlaceholder"
      @input="emit('update:modelValue', ($event.target as HTMLInputElement).value)"
      @keydown.enter="handleSearch"
    />

    <button
      v-if="filled"
      class="input-search-bar__btn"
      type="button"
      aria-label="Effacer la recherche"
      @click="handleClear"
    >
      <iconMain icon="X" size="large" />
    </button>
  </div>
</template>

<style scoped>
/* ── Wrapper ─────────────────────────────────────────────── */
.input-search-bar {
  display: inline-flex;
  align-items: center;
  width: 100%;
  height: 48px;
  border-radius: var(--radius-small);
  background: var(--interactive-input-background-default);
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--interactive-secondary-stroke-default);
  overflow: hidden;
  transition:
    background-color 150ms ease,
    box-shadow 150ms ease;
}

.input-search-bar:hover {
  box-shadow: inset 0 0 0 var(--stroke-passive) var(--text-icon-neutral-hard);
}

.input-search-bar:focus-within {
  background: var(--surface-background);
  box-shadow:
    inset 0 0 0 var(--stroke-active) var(--text-icon-neutral-hard),
    0 0 0 var(--effect-shadow-spread-small) var(--shadow-focus-focus),
    0 0 0 var(--effect-shadow-spread-medium) var(--shadow-focus-active);
}

/* ── Icon buttons (search & clear) ──────────────────────── */
.input-search-bar__btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
  padding: var(--spacing-small);
  border: none;
  border-radius: var(--radius-large);
  background: transparent;
  cursor: pointer;
  color: var(--text-icon-neutral-hard);
  transition: background-color 150ms ease;
}

.input-search-bar__btn:hover {
  background: var(--interactive-tertiary-background-rollover);
}

/* ── Text field ──────────────────────────────────────────── */
.input-search-bar__field {
  flex: 1;
  min-width: 0;
  height: 100%;
  padding: var(--spacing-small) var(--spacing-extra-extra-small);
  background: transparent;
  border: none;
  outline: none;
  font-family: 'Poppins', sans-serif;
  font-size: 14px;
  line-height: 20px;
  letter-spacing: 0;
  font-weight: 400;
  color: var(--text-icon-neutral-light);

  /* Remove browser default search input decorations */
  appearance: none;
}

.input-search-bar__field::-webkit-search-decoration,
.input-search-bar__field::-webkit-search-cancel-button {
  display: none;
}

.input-search-bar__field--filled {
  font-weight: 500;
  color: var(--text-icon-neutral-hard);
}

.input-search-bar__field::placeholder {
  font-weight: 400;
  color: var(--text-icon-neutral-light);
  opacity: 0.61;
}
</style>
