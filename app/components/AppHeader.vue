<template>
  <header class="py-6 px-4 sm:px-8 border-b border-[var(--border-subtle)] bg-[var(--bg-panel)]">
    <div class="max-w-6xl mx-auto flex items-center justify-between gap-4">
      <div>
        <h1 class="text-2xl sm:text-3xl font-bold m-0">React Native Icon Generator</h1>
        <p class="text-sm sm:text-base mt-1 mb-0 text-[var(--text-secondary)]">
          Generate Android launcher icons for your React Native app
        </p>
      </div>
      <button
        type="button"
        class="px-4 py-2 rounded-full border border-[var(--border-subtle)] bg-[var(--bg-panel)] shadow-sm text-[var(--text-primary)] font-semibold inline-flex items-center gap-2 cursor-pointer transition-all hover:bg-[var(--bg-muted)] hover:-translate-y-0.5"
        @click="toggleTheme"
      >
        <span aria-hidden="true">{{ themeIcon }}</span>
        <span class="hidden sm:inline">{{ themeLabel }}</span>
      </button>
    </div>
  </header>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const themeState = useState<'light' | 'dark'>('theme-mode', () => 'light')

const themeLabel = computed(() => (themeState.value === 'light' ? 'Dark mode' : 'Light mode'))
const themeIcon = computed(() => (themeState.value === 'light' ? '🌙' : '☀️'))

function toggleTheme() {
  themeState.value = themeState.value === 'light' ? 'dark' : 'light'
}
</script>

