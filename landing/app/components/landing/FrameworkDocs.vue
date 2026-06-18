<script setup lang="ts">
const { t } = useI18n()
const localePath = useLocalePath()

const rulesUrl = 'https://github.com/razbakov/ikigai-team/tree/main/rules'

const points = [
  { key: 'open', icon: 'lucide:git-fork' },
  { key: 'custom', icon: 'lucide:sliders-horizontal' },
  { key: 'import', icon: 'lucide:download' },
] as const
</script>

<template>
  <section class="py-20 bg-muted/30">
    <div class="max-w-5xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="text-center mb-12">
        <Icon name="lucide:book-open" class="w-10 h-10 text-primary mx-auto mb-5" />
        <h2 class="text-3xl font-bold">{{ t('frameworkDocs.title') }}</h2>
        <p class="mt-3 text-lg text-muted-foreground max-w-2xl mx-auto">
          {{ t('frameworkDocs.subtitle') }}
        </p>
      </div>

      <div class="grid sm:grid-cols-3 gap-5 mb-10">
        <div
          v-for="p in points"
          :key="p.key"
          class="bg-card border border-border rounded-xl p-6"
        >
          <Icon :name="p.icon" class="w-6 h-6 text-primary mb-3" />
          <h3 class="font-semibold mb-1">{{ t(`frameworkDocs.points.${p.key}.title`) }}</h3>
          <p class="text-sm text-muted-foreground">{{ t(`frameworkDocs.points.${p.key}.desc`) }}</p>
        </div>
      </div>

      <div class="flex flex-wrap gap-4 justify-center">
        <a
          :href="rulesUrl"
          target="_blank"
          rel="noopener"
          class="inline-flex items-center gap-2 px-8 py-4 rounded-lg bg-primary text-primary-foreground font-medium text-lg hover:opacity-90 transition-opacity"
        >
          <Icon name="lucide:scroll-text" class="w-5 h-5" />
          {{ t('frameworkDocs.rulesCta') }}
        </a>
        <NuxtLink
          :to="localePath('/guide')"
          class="inline-flex items-center gap-2 px-8 py-4 rounded-lg border border-border hover:bg-muted transition-colors text-lg"
        >
          <Icon name="lucide:compass" class="w-5 h-5" />
          {{ t('frameworkDocs.guideCta') }}
        </NuxtLink>
      </div>
    </div>
  </section>
</template>
