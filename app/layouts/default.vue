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

<style scoped>
:global(:root) {
  color-scheme: light;
  --bg-app: #f4f5f7;
  --bg-panel: #ffffff;
  --bg-surface: #f9fafb;
  --bg-muted: #eef2f6;
  --bg-muted-hover: #dbe4f0;
  --bg-input: #ffffff;
  --text-primary: #1f2933;
  --text-secondary: #64748b;
  --border-subtle: #d8e0ed;
  --accent: #2563eb;
  --accent-strong: #1d4ed8;
  --accent-soft: rgba(37, 99, 235, 0.3);
  --accent-contrast: #ffffff;
  --danger: #ef4444;
  --shadow-soft: 0 18px 32px rgba(15, 23, 42, 0.08);
  --shadow-subtle: 0 8px 18px rgba(15, 23, 42, 0.06);
  --shadow-accent: 0 12px 26px rgba(37, 99, 235, 0.25);
  --shadow-accent-strong: 0 18px 32px rgba(37, 99, 235, 0.28);
}

:global(:root[data-theme='dark']) {
  color-scheme: dark;
  --bg-app: #0f172a;
  --bg-panel: #111c34;
  --bg-surface: #13213a;
  --bg-muted: #1c2b4a;
  --bg-muted-hover: #22345a;
  --bg-input: #152338;
  --text-primary: #eff6ff;
  --text-secondary: #a3b5d9;
  --border-subtle: rgba(148, 163, 184, 0.25);
  --accent: #60a5fa;
  --accent-strong: #3b82f6;
  --accent-soft: rgba(96, 165, 250, 0.4);
  --accent-contrast: #0b1120;
  --danger: #fb7185;
  --shadow-soft: 0 18px 32px rgba(2, 6, 23, 0.55);
  --shadow-subtle: 0 8px 18px rgba(2, 6, 23, 0.45);
  --shadow-accent: 0 12px 26px rgba(59, 130, 246, 0.35);
  --shadow-accent-strong: 0 18px 32px rgba(59, 130, 246, 0.38);
}
</style>

