<script setup lang="ts">
const { t } = useI18n()

const projects = [
  { key: 'wedance', icon: 'lucide:music', color: 'text-pink-500' },
  { key: 'razbakov', icon: 'lucide:pen-tool', color: 'text-purple-500' },
  { key: 'mystery', icon: 'lucide:venetian-mask', color: 'text-amber-500' },
  { key: 'montuno', icon: 'lucide:users', color: 'text-orange-500' },
  { key: '15x4', icon: 'lucide:flask-conical', color: 'text-cyan-500' },
  { key: 'sdtv', icon: 'lucide:tv', color: 'text-green-500' },
] as const
</script>

<template>
  <section class="py-20">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="text-center mb-12">
        <h2 class="text-3xl font-bold">{{ t('socialProof.title') }}</h2>
        <p class="mt-3 text-lg text-muted-foreground max-w-2xl mx-auto">
          {{ t('socialProof.subtitle') }}
        </p>
      </div>

      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-5">
        <div
          v-for="p in projects"
          :key="p.key"
          class="bg-card border border-border rounded-xl p-6 hover:border-primary/30 transition-colors flex flex-col"
        >
          <div class="flex items-center justify-between mb-4">
            <Icon :name="p.icon" class="w-7 h-7" :class="p.color" />
            <span class="text-[10px] uppercase tracking-wide px-2 py-1 rounded-full bg-muted text-muted-foreground font-medium">
              {{ t(`socialProof.projects.${p.key}.tag`) }}
            </span>
          </div>
          <h3 class="font-semibold text-lg">{{ t(`socialProof.projects.${p.key}.name`) }}</h3>
          <p class="text-sm text-muted-foreground mb-4">{{ t(`socialProof.projects.${p.key}.desc`) }}</p>
          <div class="mt-auto flex items-center gap-2 text-sm font-medium text-primary">
            <Icon name="lucide:trending-up" class="w-4 h-4" />
            {{ t(`socialProof.projects.${p.key}.metric`) }}
          </div>
        </div>
      </div>

      <p class="text-center text-xs text-muted-foreground/70 mt-8 max-w-2xl mx-auto flex items-center justify-center gap-2">
        <Icon name="lucide:info" class="w-3.5 h-3.5 shrink-0" />
        {{ t('socialProof.disclaimer') }}
      </p>
    </div>
  </section>
</template>
