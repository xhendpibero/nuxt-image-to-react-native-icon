<template>
  <div class="max-w-6xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
    <IconGenerator />
    <AppInfo />
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'
import { useAppConfig, useHead, useSeoMeta } from '#imports'
import IconGenerator from '../features/IconGenerator.vue'
import AppInfo from '../features/AppInfo.vue'

const appConfig = useAppConfig()
const siteMeta = computed(() => appConfig.site ?? {})
const fallbackUrl = 'https://portfolio.dendysaptoadi.com/nuxt-image-to-react-native-icon'
const canonicalUrl = computed(() => {
  return typeof siteMeta.value.url === 'string' && siteMeta.value.url.length
    ? siteMeta.value.url
    : fallbackUrl
})
const absoluteOgImage = computed(() => {
  const imagePath = typeof siteMeta.value.ogImage === 'string' && siteMeta.value.ogImage.length
    ? siteMeta.value.ogImage
    : '/og-image.svg'
  try {
    return new URL(imagePath, canonicalUrl.value).toString()
  } catch {
    const base = canonicalUrl.value.replace(/\/$/, '')
    return imagePath.startsWith('/') ? `${base}${imagePath}` : `${base}/${imagePath}`
  }
})
const siteName = computed(() => {
  return typeof siteMeta.value.name === 'string' && siteMeta.value.name.length
    ? siteMeta.value.name
    : 'React Native Icon Generator'
})
const seoTitle = computed(() => `${siteName.value} | Generate Android Launcher Icons`)
const seoDescription = computed(() => {
  if (typeof siteMeta.value.description === 'string' && siteMeta.value.description.length) {
    return siteMeta.value.description
  }
  return 'Convert a single image into multiple Android launcher icon sizes for React Native apps. Generate ic_launcher.png and ic_launcher_round.png for all density folders.'
})
const schemaData = computed(() => {
  const creator =
    typeof siteMeta.value.creator === 'object' && siteMeta.value.creator
      ? siteMeta.value.creator
      : {
          name: 'Dendy Sapto Adi',
          email: 'dendysaptoadi160@gmail.com',
          sameAs: [
            'https://linkedin.com/in/dendysaptoadi',
            'https://github.com/xhendpibero',
          ],
        }
  const creatorEmail =
    typeof creator.email === 'string' && creator.email.length ? `mailto:${creator.email}` : undefined
  return [
    {
      '@context': 'https://schema.org',
      '@type': 'WebApplication',
      name: siteName.value,
      description: seoDescription.value,
      url: canonicalUrl.value,
      operatingSystem: 'Web',
      applicationCategory: 'MultimediaApplication',
      image: absoluteOgImage.value,
      inLanguage: 'en',
      offers: {
        '@type': 'Offer',
        price: '0',
        priceCurrency: 'USD',
      },
      creator: {
        '@type': 'Person',
        name: creator.name,
        ...(creatorEmail ? { email: creatorEmail } : {}),
        sameAs: Array.isArray(creator.sameAs) ? creator.sameAs : [],
      },
      potentialAction: {
        '@type': 'Action',
        name: 'Generate React Native Android launcher icons',
        target: [
          {
            '@type': 'EntryPoint',
            urlTemplate: canonicalUrl.value,
          },
        ],
      },
    },
  ]
})

useSeoMeta(() => ({
  title: seoTitle.value,
  ogTitle: seoTitle.value,
  description: seoDescription.value,
  ogDescription: seoDescription.value,
  ogType: 'website',
  ogUrl: canonicalUrl.value,
  ogSiteName: siteMeta.value.shortName ?? siteMeta.value.name ?? 'RN Icon Gen',
  ogImage: absoluteOgImage.value,
  twitterCard: 'summary_large_image',
  twitterTitle: seoTitle.value,
  twitterDescription: seoDescription.value,
  twitterImage: absoluteOgImage.value,
}))

useHead(() => ({
  link: [
    {
      rel: 'canonical',
      href: canonicalUrl.value,
    },
  ],
  meta: [
    {
      name: 'application-name',
      content: siteMeta.value.shortName ?? siteMeta.value.name ?? 'RN Icon Gen',
    },
    {
      name: 'apple-mobile-web-app-title',
      content: siteMeta.value.shortName ?? siteMeta.value.name ?? 'RN Icon Gen',
    },
    {
      name: 'theme-color',
      content: '#2563eb',
    },
  ],
  script: [
    {
      key: 'schema-org-webapp',
      type: 'application/ld+json',
      children: JSON.stringify(schemaData.value),
    },
  ],
}))
</script>

