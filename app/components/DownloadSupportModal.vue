<template>
  <div
    v-if="modelValue"
    class="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/50 backdrop-blur-sm"
    @click.self="close"
  >
    <div class="modal-content rounded-2xl max-w-lg w-full p-6 sm:p-8">
      <div class="mb-6">
        <h3 class="text-2xl font-bold mb-2">Support the Creator</h3>
        <p class="text-sm leading-relaxed modal-text-secondary">
          This tool is free to use. If you find it helpful, please consider supporting the creator to help keep this project running and improve it further.
        </p>
      </div>

      <div class="mb-6 p-4 rounded-xl border modal-surface modal-border">
        <p class="text-sm modal-text-secondary mb-3">
          <strong class="modal-text-primary">You're using the free plan</strong>
        </p>
        <p class="text-xs modal-text-secondary mb-4">
          The free plan lets you use this tool for personal use. Support the creator to unlock premium features:
        </p>
        <ul class="space-y-2 text-xs modal-text-secondary">
          <li class="flex items-start gap-2">
            <svg class="w-4 h-4 modal-accent mt-0.5 flex-shrink-0" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
            </svg>
            <span>Commercial Use License</span>
          </li>
          <li class="flex items-start gap-2">
            <svg class="w-4 h-4 modal-accent mt-0.5 flex-shrink-0" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
            </svg>
            <span>No Wait Time - Instant Downloads</span>
          </li>
          <li class="flex items-start gap-2">
            <svg class="w-4 h-4 modal-accent mt-0.5 flex-shrink-0" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
            </svg>
            <span>Unlimited Usage</span>
          </li>
          <li class="flex items-start gap-2">
            <svg class="w-4 h-4 modal-accent mt-0.5 flex-shrink-0" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
            </svg>
            <span>No Ads</span>
          </li>
          <li class="flex items-start gap-2">
            <svg class="w-4 h-4 modal-accent mt-0.5 flex-shrink-0" fill="currentColor" viewBox="0 0 20 20">
              <path fill-rule="evenodd" d="M10 18a8 8 0 100-16 8 8 0 000 16zm3.707-9.293a1 1 0 00-1.414-1.414L9 10.586 7.707 9.293a1 1 0 00-1.414 1.414l2 2a1 1 0 001.414 0l4-4z" clip-rule="evenodd" />
            </svg>
            <span>Priority Support</span>
          </li>
        </ul>
      </div>

      <div class="flex flex-col sm:flex-row gap-3">
        <button
          type="button"
          class="flex-1 px-6 py-3 rounded-full font-semibold transition-all modal-button-primary"
          @click="handleDownload"
        >
          Download ZIP
        </button>
        <a
          href="https://tako.id/example"
          target="_blank"
          rel="noopener noreferrer"
          class="flex-1 px-6 py-3 rounded-full border-2 font-semibold transition-all text-center modal-button-secondary"
          @click="close"
        >
          ☕ Give Coffee
        </a>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  modelValue: boolean
}>()

const emit = defineEmits<{
  'update:modelValue': [value: boolean]
  'download': []
}>()

function close() {
  emit('update:modelValue', false)
}

function handleDownload() {
  emit('download')
  close()
}
</script>

<style scoped>
.modal-content {
  background: var(--bg-panel);
  box-shadow: var(--shadow-soft);
  border: 1px solid var(--border-subtle);
}

.modal-text-primary {
  color: var(--text-primary);
}

.modal-text-secondary {
  color: var(--text-secondary);
}

.modal-accent {
  color: var(--accent);
}

.modal-surface {
  background: var(--bg-surface);
}

.modal-border {
  border-color: var(--border-subtle);
}

.modal-button-primary {
  background: linear-gradient(135deg, var(--accent), var(--accent-strong));
  color: var(--accent-contrast);
  box-shadow: var(--shadow-accent);
}

.modal-button-primary:hover {
  box-shadow: var(--shadow-accent-strong);
  transform: translateY(-2px);
}

.modal-button-secondary {
  border-color: var(--accent);
  color: var(--accent);
}

.modal-button-secondary:hover {
  background: var(--accent-soft);
}
</style>

