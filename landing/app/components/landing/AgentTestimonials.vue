<script setup lang="ts">
const { t } = useI18n()

const items = [
  { id: 'forge', name: 'Forge', role: 'CTO', avatar: '/images/agents/viktor.png', color: 'var(--color-forge)' },
  { id: 'north', name: 'North', role: 'Head of Strategy & Business', avatar: '/images/agents/marco.png', color: 'var(--color-north)' },
  { id: 'bridge', name: 'Bridge', role: 'Community & Partnerships', avatar: '/images/agents/kai.png', color: 'var(--color-bridge)' },
] as const
</script>

<template>
  <section class="py-20">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="text-center mb-12">
        <h2 class="text-3xl font-bold">{{ t('testimonials.title') }}</h2>
        <p class="mt-3 text-lg text-muted-foreground max-w-2xl mx-auto">
          {{ t('testimonials.subtitle') }}
        </p>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        <figure
          v-for="item in items"
          :key="item.id"
          class="bg-card border border-border rounded-2xl p-6 flex flex-col"
        >
          <Icon name="lucide:quote" class="w-7 h-7 text-primary/40 mb-4" />
          <blockquote class="text-sm leading-relaxed text-foreground/90 mb-6 grow">
            {{ t(`testimonials.items.${item.id}.quote`) }}
          </blockquote>
          <div
            class="text-xs font-medium mb-5 inline-flex items-center gap-2 self-start px-3 py-1.5 rounded-full"
            :style="{ background: `color-mix(in oklch, ${item.color} 14%, transparent)`, color: item.color }"
          >
            <Icon name="lucide:check-circle" class="w-3.5 h-3.5" />
            {{ t(`testimonials.items.${item.id}.metric`) }}
          </div>
          <figcaption class="flex items-center gap-3 pt-4 border-t border-border">
            <img
              :src="item.avatar"
              :alt="item.name"
              class="w-10 h-10 rounded-full object-cover ring-2"
              :style="{ '--tw-ring-color': item.color }"
            />
            <div>
              <div class="font-semibold text-sm">{{ item.name }}</div>
              <div class="text-xs text-muted-foreground">{{ item.role }}</div>
            </div>
            <span class="ml-auto text-[10px] uppercase tracking-wide text-muted-foreground">
              {{ t('testimonials.quarterLabel') }}
            </span>
          </figcaption>
        </figure>
      </div>
    </div>
  </section>
</template>
