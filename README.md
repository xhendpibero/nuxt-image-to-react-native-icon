# React Native Icon Generator

A Nuxt.js web application that converts a single image into multiple Android launcher icon sizes for React Native apps. Generate `ic_launcher.png` and `ic_launcher_round.png` for all density folders (mdpi, hdpi, xhdpi, xxhdpi, xxxhdpi).

## Features

- 🎨 **Single Image Input**: Upload one image and generate all required sizes
- 📱 **Android Compatible**: Generates icons for all Android density folders
- 🔄 **Dual Formats**: Creates both square (`ic_launcher.png`) and round (`ic_launcher_round.png`) icons
- 🔒 **Privacy First**: All processing happens in your browser - no server uploads
- 📦 **ZIP Download**: Download all generated icons in a ready-to-use folder structure
- 🎯 **Multiple Formats**: Supports PNG, JPG, JPEG, and WEBP input formats
- 📱 **Responsive UI**: Beautiful, minimalist interface built with Tailwind CSS

## Icon Sizes Generated

- **mipmap-mdpi**: 48×48 pixels
- **mipmap-hdpi**: 72×72 pixels
- **mipmap-xhdpi**: 96×96 pixels
- **mipmap-xxhdpi**: 144×144 pixels
- **mipmap-xxxhdpi**: 192×192 pixels

## Getting Started

### Installation

```bash
npm install
```

### Development

```bash
npm run dev
```

### Build

```bash
npm run build
```

### Preview

```bash
npm run preview
```

## Usage

1. Upload your source image (PNG, JPG, JPEG, or WEBP)
2. Review the preview and icon sizes that will be generated
3. Click "Generate Icons" to process your image
4. Download the ZIP file containing all icon folders
5. Extract and copy the `res` folder to your React Native project's `android/app/src/main/` directory

## Project Structure

```
nuxt-image-to-react-native-icon/
├── app/
│   ├── components/       # Reusable components (Header, Footer)
│   ├── features/         # Main feature components
│   │   ├── IconGenerator.vue  # Main icon generation feature
│   │   └── AppInfo.vue        # Informational sections
│   ├── layouts/          # Layout components
│   └── pages/            # Page components
├── assets/
│   └── css/              # Global styles and Tailwind
└── public/               # Static assets
```

## Technologies

- **Nuxt 4**: Vue.js framework
- **Tailwind CSS**: Utility-first CSS framework
- **JSZip**: Client-side ZIP file generation
- **TypeScript**: Type safety

## License

MIT

