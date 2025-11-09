<template>
  <div class="min-h-screen flex flex-col bg-[var(--bg-app)] text-[var(--text-primary)]">
    <AppHeader />
    <main class="flex-1">
      <slot />
    </main>
    <AppFooter />
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, watch } from 'vue'

type ThemeMode = 'light' | 'dark'

const THEME_STORAGE_KEY = 'nuxt-rn-icon-theme'

const themeState = useState<ThemeMode>('theme-mode', () => 'light')

const theme = computed({
  get: () => themeState.value,
  set: (value:ThemeMode) => {
    themeState.value = value
  },
})

function applyTheme(value: ThemeMode) {
  if (process.client) {
    document.documentElement.dataset.theme = value
    localStorage.setItem(THEME_STORAGE_KEY, value)
  }
}

function detectPreferredTheme(): ThemeMode {
  if (!process.client) {
    return 'light'
  }
  const stored = localStorage.getItem(THEME_STORAGE_KEY) as ThemeMode | null
  if (stored === 'light' || stored === 'dark') {
    return stored
  }
  return window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches
    ? 'dark'
    : 'light'
}

onMounted(() => {
  const initial = detectPreferredTheme()
  theme.value = initial
  applyTheme(initial)
})

watch(
  theme,
  (value) => {
    applyTheme(value)
  },
  { immediate: false },
)
</script>

