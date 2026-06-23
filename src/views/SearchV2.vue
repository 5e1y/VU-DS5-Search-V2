<script setup lang="ts">
import websiteHeader from '../components/websiteHeader.vue'
import WebsiteSearch from './WebsiteSearch.vue'
</script>

<template>
  <div data-theme="Desktop">
    <websiteHeader>
      <template #search-overlay="{ open, close, origin }">
        <WebsiteSearch :open="open" :origin="origin" @close="close" />
      </template>
    </websiteHeader>
  </div>
</template>
