<script setup lang="ts">
import buttonMain from './buttonMain.vue'

type SearchResultType = 'past' | 'present'

withDefaults(
  defineProps<{
    text?: string
    type?: SearchResultType
    image?: string
  }>(),
  {
    text: '',
    type: 'past',
  },
)

const emit = defineEmits<{
  click: []
  clear: []
}>()
</script>

<template>
  <div
    class="search-result"
    :class="`search-result--${type}`"
    role="button"
    tabindex="0"
    @click="emit('click')"
    @keydown.enter="emit('click')"
  >
    <span v-if="type === 'past' && image" class="search-result__thumb">
      <img :src="image" alt="" />
    </span>

    <span class="search-result__text" :class="type === 'present' ? 'normal-300' : 'medium-200'">
      <slot>{{ text }}</slot>
    </span>

    <buttonMain
      v-if="type === 'past'"
      class="search-result__clear"
      type="ghost"
      size="small"
      icon-left="X"
      aria-label="Supprimer cette recherche"
      @click.stop="emit('clear')"
    />
  </div>
</template>

<style scoped>
.search-result {
  display: flex;
  align-items: center;
  width: 100%;
  cursor: pointer;
  color: var(--text-icon-neutral-medium);
}

/* ── Variante "past" (anciennes recherches) ── */
.search-result--past {
  gap: var(--spacing-extra-small);
  padding: var(--spacing-extra-small);
  border-radius: var(--radius-medium);
  background: var(--interactive-tertiary-background-default);
}
.search-result--past:hover {
  background: var(--interactive-tertiary-background-rollover);
}

/* ── Variante "present" (suggestions de saisie) ── */
.search-result--present {
  padding: var(--spacing-small);
  border-radius: var(--radius-small);
  background: var(--interactive-ghost-background-default);
}
.search-result--present:hover {
  background: var(--interactive-ghost-background-rollover);
}

.search-result__thumb {
  flex-shrink: 0;
  width: 64px;
  height: 48px;
  overflow: clip;
  border-radius: var(--radius-small);
}
.search-result__thumb img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.search-result__text {
  flex: 1;
  min-width: 0;
  text-align: left;
  word-break: break-word;
}

/* Bouton ghost d'effacement : visible au survol/focus de la ligne. */
.search-result__clear {
  flex-shrink: 0;
  opacity: 0;
  transition: opacity 150ms ease;
}
.search-result:hover .search-result__clear,
.search-result:focus-within .search-result__clear {
  opacity: 1;
}
</style>
