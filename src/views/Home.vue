<script setup lang="ts">
import { useRouter } from 'vue-router'
import buttonMain from '../components/buttonMain.vue'

const router = useRouter()

// Liste des prototypes en cours — ajouter une entrée par test.
const tests = [{ label: 'Recherche V2', to: '/recherche-v2' }]
</script>

<template>
  <main class="home">
    <div class="home__list">
      <buttonMain
        v-for="t in tests"
        :key="t.to"
        :label="t.label"
        type="primary"
        @click="router.push(t.to)"
      />
    </div>
  </main>
</template>

<style scoped>
.home {
  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: var(--spacing-large);
}
.home__list {
  display: flex;
  flex-direction: column;
  gap: var(--spacing-small);
}
</style>
