<template>
  <section class="space-y-8">
    <!-- About Section -->
    <div class="bg-[var(--bg-panel)] rounded-2xl shadow-[var(--shadow-soft)] border border-[var(--border-subtle)] p-6 sm:p-8">
      <h2 class="text-2xl font-bold mb-4">What is this tool?</h2>
      <div class="prose prose-sm max-w-none text-[var(--text-secondary)] space-y-4">
        <p>
          This tool helps React Native developers quickly generate all required Android launcher icon sizes from a single source image. 
          Instead of manually creating icons for each density folder (mdpi, hdpi, xhdpi, xxhdpi, xxxhdpi), you can upload one image and 
          get a complete set of icons ready to use in your React Native project.
        </p>
        <p>
          The tool generates both <code class="px-1.5 py-0.5 bg-[var(--bg-muted)] rounded">ic_launcher.png</code> (square) and 
          <code class="px-1.5 py-0.5 bg-[var(--bg-muted)] rounded">ic_launcher_round.png</code> (circular) for each density folder, 
          following Android's standard icon requirements.
        </p>
        <p class="mb-0">
          All processing happens entirely in your browser - your images never leave your device, ensuring complete privacy and security.
        </p>
      </div>
    </div>

    <!-- How to Use Section -->
    <div class="bg-[var(--bg-panel)] rounded-2xl shadow-[var(--shadow-soft)] border border-[var(--border-subtle)] p-6 sm:p-8">
      <h2 class="text-2xl font-bold mb-4">How to Use</h2>
      <ol class="space-y-3 text-[var(--text-secondary)]">
        <li class="flex gap-3">
          <span class="flex-shrink-0 w-6 h-6 rounded-full bg-[var(--accent-soft)] text-[var(--accent)] font-bold flex items-center justify-center text-sm">1</span>
          <span><strong class="text-[var(--text-primary)]">Upload your image:</strong> Click the upload area or drag and drop your source image. Supported formats: PNG, JPG, JPEG, WEBP (max 10MB).</span>
        </li>
        <li class="flex gap-3">
          <span class="flex-shrink-0 w-6 h-6 rounded-full bg-[var(--accent-soft)] text-[var(--accent)] font-bold flex items-center justify-center text-sm">2</span>
          <span><strong class="text-[var(--text-primary)]">Review the preview:</strong> Check your image and see the list of icon sizes that will be generated.</span>
        </li>
        <li class="flex gap-3">
          <span class="flex-shrink-0 w-6 h-6 rounded-full bg-[var(--accent-soft)] text-[var(--accent)] font-bold flex items-center justify-center text-sm">3</span>
          <span><strong class="text-[var(--text-primary)]">Generate icons:</strong> Click the "Generate Icons" button to process your image and create all required sizes.</span>
        </li>
        <li class="flex gap-3">
          <span class="flex-shrink-0 w-6 h-6 rounded-full bg-[var(--accent-soft)] text-[var(--accent)] font-bold flex items-center justify-center text-sm">4</span>
          <span><strong class="text-[var(--text-primary)]">Download:</strong> Once processing is complete, download the ZIP file containing all icon folders.</span>
        </li>
        <li class="flex gap-3">
          <span class="flex-shrink-0 w-6 h-6 rounded-full bg-[var(--accent-soft)] text-[var(--accent)] font-bold flex items-center justify-center text-sm">5</span>
          <span><strong class="text-[var(--text-primary)]">Extract and use:</strong> Extract the ZIP file and copy the <code class="px-1.5 py-0.5 bg-[var(--bg-muted)] rounded text-xs">res</code> folder to your React Native project's <code class="px-1.5 py-0.5 bg-[var(--bg-muted)] rounded text-xs">android/app/src/main/</code> directory.</span>
        </li>
      </ol>
    </div>

    <!-- FAQ Section -->
    <div class="bg-[var(--bg-panel)] rounded-2xl shadow-[var(--shadow-soft)] border border-[var(--border-subtle)] p-6 sm:p-8">
      <h2 class="text-2xl font-bold mb-4">Frequently Asked Questions</h2>
      <div class="space-y-6">
        <div>
          <h3 class="font-semibold text-lg mb-2 text-[var(--text-primary)]">What image formats are supported?</h3>
          <p class="text-[var(--text-secondary)]">
            We support PNG, JPG, JPEG, and WEBP formats. The output will always be PNG format to ensure compatibility with Android requirements.
          </p>
        </div>
        <div>
          <h3 class="font-semibold text-lg mb-2 text-[var(--text-primary)]">What are the icon sizes generated?</h3>
          <p class="text-[var(--text-secondary)] mb-2">
            The tool generates icons in the following sizes:
          </p>
          <ul class="list-disc list-inside space-y-1 text-[var(--text-secondary)] ml-4">
            <li><strong>mipmap-mdpi:</strong> 48×48 pixels</li>
            <li><strong>mipmap-hdpi:</strong> 72×72 pixels</li>
            <li><strong>mipmap-xhdpi:</strong> 96×96 pixels</li>
            <li><strong>mipmap-xxhdpi:</strong> 144×144 pixels</li>
            <li><strong>mipmap-xxxhdpi:</strong> 192×192 pixels</li>
          </ul>
        </div>
        <div>
          <h3 class="font-semibold text-lg mb-2 text-[var(--text-primary)]">What's the difference between ic_launcher.png and ic_launcher_round.png?</h3>
          <p class="text-[var(--text-secondary)]">
            <code class="px-1.5 py-0.5 bg-[var(--bg-muted)] rounded">ic_launcher.png</code> is a square icon, while 
            <code class="px-1.5 py-0.5 bg-[var(--bg-muted)] rounded">ic_launcher_round.png</code> is a circular version. 
            Android devices can use either format depending on the device's launcher preferences.
          </p>
        </div>
        <div>
          <h3 class="font-semibold text-lg mb-2 text-[var(--text-primary)]">Is my image data secure?</h3>
          <p class="text-[var(--text-secondary)]">
            Yes! All image processing happens entirely in your browser using client-side JavaScript. Your images are never uploaded to any server, 
            ensuring complete privacy and security.
          </p>
        </div>
        <div>
          <h3 class="font-semibold text-lg mb-2 text-[var(--text-primary)]">Can I use this for iOS icons?</h3>
          <p class="text-[var(--text-secondary)]">
            This tool is specifically designed for Android launcher icons. For iOS icons, you'll need different sizes and formats. 
            However, you can use the generated square icons as a starting point for iOS if needed.
          </p>
        </div>
        <div>
          <h3 class="font-semibold text-lg mb-2 text-[var(--text-primary)]">What should be the ideal source image size?</h3>
          <p class="text-[var(--text-secondary)]">
            For best results, use a square source image (1:1 aspect ratio) with at least 1024×1024 pixels. 
            Larger images will provide better quality when scaled down to smaller sizes. The image should have a transparent or solid background.
          </p>
        </div>
        <div>
          <h3 class="font-semibold text-lg mb-2 text-[var(--text-primary)]">Where do I place the generated icons in my React Native project?</h3>
          <p class="text-[var(--text-secondary)]">
            Extract the ZIP file and copy the entire <code class="px-1.5 py-0.5 bg-[var(--bg-muted)] rounded">res</code> folder to 
            <code class="px-1.5 py-0.5 bg-[var(--bg-muted)] rounded">android/app/src/main/</code> in your React Native project. 
            Make sure to replace any existing <code class="px-1.5 py-0.5 bg-[var(--bg-muted)] rounded">res</code> folder if it exists.
          </p>
        </div>
      </div>
    </div>
  </section>
</template>

