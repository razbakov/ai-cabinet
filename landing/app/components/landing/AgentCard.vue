<script setup lang="ts">
import type { AgentDefinition } from '~/types/agent'

const { t, tm, rt } = useI18n()

const props = defineProps<{
  agent: AgentDefinition
}>()

const colorMap: Record<string, string> = {
  maya: 'border-t-[var(--color-maya)] bg-[var(--color-maya)]/5',
  forge: 'border-t-[var(--color-forge)] bg-[var(--color-forge)]/5',
  torch: 'border-t-[var(--color-torch)] bg-[var(--color-torch)]/5',
  north: 'border-t-[var(--color-north)] bg-[var(--color-north)]/5',
  sage: 'border-t-[var(--color-sage)] bg-[var(--color-sage)]/5',
  bridge: 'border-t-[var(--color-bridge)] bg-[var(--color-bridge)]/5',
  oracle: 'border-t-[var(--color-oracle)] bg-[var(--color-oracle)]/5',
}

const ringColorMap: Record<string, string> = {
  maya: 'ring-[var(--color-maya)]',
  forge: 'ring-[var(--color-forge)]',
  torch: 'ring-[var(--color-torch)]',
  north: 'ring-[var(--color-north)]',
  sage: 'ring-[var(--color-sage)]',
  bridge: 'ring-[var(--color-bridge)]',
  oracle: 'ring-[var(--color-oracle)]',
}

const responsibilities = computed(() => {
  const raw = tm(`agents.${props.agent.id}.responsibilities`)
  return Array.isArray(raw) ? raw.map(r => rt(r)) : props.agent.responsibilities
})
</script>

<template>
  <div
    class="rounded-xl border border-border border-t-4 p-6 transition-all hover:shadow-lg hover:-translate-y-1"
    :class="colorMap[agent.color]"
  >
    <div class="flex items-start justify-between mb-4">
      <div class="flex items-center gap-3">
        <img
          :src="agent.avatar"
          :alt="agent.name"
          class="w-12 h-12 rounded-full object-cover ring-2"
          :class="ringColorMap[agent.color]"
        />
        <div>
          <h3 class="font-semibold text-lg">{{ agent.name }}</h3>
          <p class="text-sm text-muted-foreground">{{ t(`agents.${agent.id}.role`) }}</p>
        </div>
      </div>
      <span class="text-xs px-2 py-1 rounded-full bg-muted text-muted-foreground font-mono">
        {{ agent.personality }}
      </span>
    </div>
    <p class="text-sm text-muted-foreground mb-4 line-clamp-2">
      {{ t(`agents.${agent.id}.description`) }}
    </p>
    <ul class="space-y-2">
      <li
        v-for="resp in responsibilities"
        :key="resp"
        class="flex items-start gap-2 text-sm"
      >
        <Icon name="lucide:check" class="w-4 h-4 mt-0.5 shrink-0 text-primary" />
        <span>{{ resp }}</span>
      </li>
    </ul>
  </div>
</template>
