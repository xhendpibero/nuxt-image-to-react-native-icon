<template>
  <section class="bg-[var(--bg-panel)] rounded-2xl shadow-[var(--shadow-soft)] border border-[var(--border-subtle)] p-6 sm:p-8 mb-8">
    <div class="mb-6">
      <h2 class="text-2xl sm:text-3xl font-bold mb-2">Icon Generator</h2>
      <p class="text-[var(--text-secondary)] text-sm sm:text-base">
        Upload a single image and generate all required Android launcher icon sizes for your React Native app.
      </p>
    </div>

    <div
      v-if="!selectedFile && !isProcessing"
      class="border-2 border-dashed rounded-xl p-12 text-center transition-colors"
      :class="isDragging ? 'border-[var(--accent)] bg-[var(--accent-soft)]' : 'border-[var(--border-subtle)] bg-[var(--bg-surface)]'"
      @dragover.prevent="onDragOver"
      @dragleave.prevent="onDragLeave"
      @drop.prevent="onDrop"
    >
      <label class="cursor-pointer">
        <input
          type="file"
          accept="image/png,image/jpeg,image/jpg,image/webp"
          class="hidden"
          @change="onFileSelected"
        />
        <div class="flex flex-col items-center gap-4">
          <svg class="w-16 h-16 text-[var(--text-secondary)]" fill="none" stroke="currentColor" viewBox="0 0 24 24">
            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12" />
          </svg>
          <div>
            <p class="text-lg font-semibold mb-1">Click to upload or drag and drop</p>
            <p class="text-sm text-[var(--text-secondary)]">PNG, JPG, JPEG, or WEBP (Max 10MB)</p>
          </div>
        </div>
      </label>
    </div>

    <div v-if="selectedFile && !isProcessing" class="space-y-6">
      <div class="flex flex-col sm:flex-row gap-4 items-start sm:items-center justify-between p-4 bg-[var(--bg-surface)] rounded-xl border border-[var(--border-subtle)]">
        <div class="flex items-center gap-4">
          <img
            v-if="previewUrl"
            :src="previewUrl"
            :alt="selectedFile.name"
            class="w-20 h-20 object-cover rounded-lg"
          />
          <div>
            <p class="font-semibold mb-1">{{ selectedFile.name }}</p>
            <p class="text-sm text-[var(--text-secondary)]">
              {{ formatFilesize(selectedFile.size) }}
              <span v-if="imageDimensions"> · {{ imageDimensions.width }} × {{ imageDimensions.height }} px</span>
            </p>
          </div>
        </div>
        <button
          type="button"
          class="px-4 py-2 rounded-lg border border-[var(--border-subtle)] bg-[var(--bg-panel)] text-[var(--text-primary)] font-semibold hover:bg-[var(--bg-muted)] transition-colors"
          @click="clearFile"
        >
          Change Image
        </button>
      </div>

      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
        <div
          v-for="size in iconSizes"
          :key="size.folder"
          class="p-4 border border-[var(--border-subtle)] rounded-xl bg-[var(--bg-surface)]"
        >
          <div class="flex items-center justify-between mb-2">
            <h3 class="font-semibold text-sm">{{ size.folder }}</h3>
            <span class="text-xs text-[var(--text-secondary)]">{{ size.size }}×{{ size.size }}px</span>
          </div>
          <div class="text-xs text-[var(--text-secondary)] space-y-1">
            <p>• ic_launcher.png</p>
            <p>• ic_launcher_round.png</p>
          </div>
        </div>
      </div>

      <button
        type="button"
        class="w-full py-3 px-6 rounded-full bg-gradient-to-r from-[var(--accent)] to-[var(--accent-strong)] text-[var(--accent-contrast)] font-semibold shadow-[var(--shadow-accent)] hover:shadow-[var(--shadow-accent-strong)] hover:-translate-y-0.5 transition-all disabled:opacity-60 disabled:cursor-not-allowed disabled:hover:translate-y-0"
        :disabled="!selectedFile"
        @click="processImage"
      >
        Generate Icons
      </button>
    </div>

    <div v-if="isProcessing" class="text-center py-12">
      <div class="inline-block w-12 h-12 border-4 border-[var(--accent-soft)] border-t-[var(--accent)] rounded-full animate-spin mb-4"></div>
      <p class="text-lg font-semibold mb-2">Processing your image...</p>
      <p class="text-sm text-[var(--text-secondary)]">Generating icons for all density folders</p>
    </div>

    <div v-if="zipResult" class="mt-6 p-6 bg-gradient-to-r from-[var(--accent-soft)] to-transparent border border-[var(--accent-soft)] rounded-xl">
      <div class="flex flex-col sm:flex-row items-start sm:items-center justify-between gap-4">
        <div>
          <h3 class="font-semibold text-lg mb-1">Icons Generated Successfully!</h3>
          <p class="text-sm text-[var(--text-secondary)]">Archive size: {{ formatFilesize(zipResult.size) }}</p>
        </div>
        <a
          :href="zipResult.url"
          download="react-native-icons.zip"
          class="px-6 py-3 rounded-full bg-gradient-to-r from-[var(--accent)] to-[var(--accent-strong)] text-[var(--accent-contrast)] font-semibold shadow-[var(--shadow-accent)] hover:shadow-[var(--shadow-accent-strong)] hover:-translate-y-0.5 transition-all inline-block"
        >
          Download ZIP
        </a>
      </div>
    </div>

    <p v-if="error" class="mt-4 text-[var(--danger)] font-semibold">{{ error }}</p>
  </section>
</template>

<script setup lang="ts">
import { ref, onBeforeUnmount } from 'vue'
import JSZip from 'jszip'

type IconSize = {
  folder: string
  size: number
}

const iconSizes: IconSize[] = [
  { folder: 'mipmap-mdpi', size: 48 },
  { folder: 'mipmap-hdpi', size: 72 },
  { folder: 'mipmap-xhdpi', size: 96 },
  { folder: 'mipmap-xxhdpi', size: 144 },
  { folder: 'mipmap-xxxhdpi', size: 192 },
]

const selectedFile = ref<File | null>(null)
const previewUrl = ref<string | null>(null)
const imageDimensions = ref<{ width: number; height: number } | null>(null)
const isProcessing = ref(false)
const zipResult = ref<{ url: string; size: number } | null>(null)
const error = ref<string | null>(null)
const isDragging = ref(false)

function processFile(file: File) {
  // Validate file type
  const validTypes = ['image/png', 'image/jpeg', 'image/jpg', 'image/webp']
  if (!validTypes.includes(file.type)) {
    error.value = 'Please select a valid image file (PNG, JPG, JPEG, or WEBP)'
    return
  }

  // Validate file size (10MB max)
  if (file.size > 10 * 1024 * 1024) {
    error.value = 'File size must be less than 10MB'
    return
  }

  error.value = null
  selectedFile.value = file
  zipResult.value = null

  // Create preview
  if (previewUrl.value) {
    URL.revokeObjectURL(previewUrl.value)
  }
  previewUrl.value = URL.createObjectURL(file)

  // Get image dimensions
  loadImageDimensions(previewUrl.value)
}

function onFileSelected(event: Event) {
  const input = event.target as HTMLInputElement
  const file = input.files?.[0]
  if (!file) return
  processFile(file)
}

function onDragOver(event: DragEvent) {
  event.preventDefault()
  isDragging.value = true
}

function onDragLeave(event: DragEvent) {
  event.preventDefault()
  // Only set isDragging to false if we're leaving the drop zone
  const target = event.currentTarget as HTMLElement
  const relatedTarget = event.relatedTarget as HTMLElement
  if (!target.contains(relatedTarget)) {
    isDragging.value = false
  }
}

function onDrop(event: DragEvent) {
  event.preventDefault()
  isDragging.value = false

  const files = event.dataTransfer?.files
  if (!files || files.length === 0) return

  const file = files[0]
  processFile(file)
}

async function loadImageDimensions(url: string) {
  try {
    const img = await loadImage(url)
    imageDimensions.value = {
      width: img.naturalWidth || img.width,
      height: img.naturalHeight || img.height,
    }
  } catch (err) {
    console.error('Failed to load image dimensions:', err)
  }
}

function loadImage(source: string): Promise<HTMLImageElement> {
  return new Promise((resolve, reject) => {
    const image = new Image()
    image.onload = () => resolve(image)
    image.onerror = () => reject(new Error('Failed to load image.'))
    image.src = source
  })
}

function clearFile() {
  if (previewUrl.value) {
    URL.revokeObjectURL(previewUrl.value)
  }
  selectedFile.value = null
  previewUrl.value = null
  imageDimensions.value = null
  zipResult.value = null
  error.value = null
}

async function processImage() {
  if (!selectedFile.value || !previewUrl.value) {
    error.value = 'Please select an image first'
    return
  }

  error.value = null
  isProcessing.value = true
  zipResult.value = null

  try {
    const image = await loadImage(previewUrl.value)
    const zip = new JSZip()

    // Process each icon size
    for (const iconSize of iconSizes) {
      const folder = zip.folder(iconSize.folder)
      if (!folder) continue

      // Generate ic_launcher.png (square)
      const squareBlob = await resizeImage(image, iconSize.size, iconSize.size, false)
      folder.file('ic_launcher.png', squareBlob)

      // Generate ic_launcher_round.png (round)
      const roundBlob = await resizeImage(image, iconSize.size, iconSize.size, true)
      folder.file('ic_launcher_round.png', roundBlob)
    }

    // Generate ZIP
    const zipBlob = await zip.generateAsync({ type: 'blob' })
    zipResult.value = {
      url: URL.createObjectURL(zipBlob),
      size: zipBlob.size,
    }
  } catch (err) {
    console.error('Processing error:', err)
    error.value = err instanceof Error ? err.message : 'Failed to process image. Please try again.'
  } finally {
    isProcessing.value = false
  }
}

async function resizeImage(
  image: HTMLImageElement,
  width: number,
  height: number,
  round: boolean
): Promise<Blob> {
  const canvas = document.createElement('canvas')
  canvas.width = width
  canvas.height = height
  const ctx = canvas.getContext('2d')
  if (!ctx) {
    throw new Error('Canvas context unavailable')
  }

  if (round) {
    // For round icons, create a circular clipping path first
    ctx.beginPath()
    ctx.arc(width / 2, height / 2, width / 2, 0, Math.PI * 2)
    ctx.clip()
  }

  // Draw image (will be clipped to circle if round is true)
  ctx.drawImage(image, 0, 0, width, height)

  return new Promise((resolve, reject) => {
    canvas.toBlob(
      (blob) => {
        if (!blob) {
          reject(new Error('Failed to generate image blob'))
          return
        }
        resolve(blob)
      },
      'image/png',
      1.0
    )
  })
}

function formatFilesize(size: number): string {
  if (size < 1024) {
    return `${size} B`
  }
  if (size < 1024 * 1024) {
    return `${(size / 1024).toFixed(1)} KB`
  }
  return `${(size / (1024 * 1024)).toFixed(2)} MB`
}

onBeforeUnmount(() => {
  if (previewUrl.value) {
    URL.revokeObjectURL(previewUrl.value)
  }
  if (zipResult.value) {
    URL.revokeObjectURL(zipResult.value.url)
  }
})
</script>

