<script setup lang="ts">
const { t } = useI18n()

// 12 domains in wheel order, each with its steward + accent color
const domains = [
  { key: 'spirit', steward: 'Oracle', color: 'oklch(0.6 0.18 200)' },
  { key: 'body', steward: 'Sage', color: 'oklch(0.55 0.2 310)' },
  { key: 'mind', steward: 'Sage', color: 'oklch(0.55 0.2 310)' },
  { key: 'family', steward: 'Bridge', color: 'oklch(0.65 0.18 55)' },
  { key: 'friends', steward: 'Bridge', color: 'oklch(0.65 0.18 55)' },
  { key: 'community', steward: 'Bridge', color: 'oklch(0.65 0.18 55)' },
  { key: 'business', steward: 'North', color: 'oklch(0.65 0.15 80)' },
  { key: 'work', steward: 'North', color: 'oklch(0.65 0.15 80)' },
  { key: 'money', steward: 'North', color: 'oklch(0.65 0.15 80)' },
  { key: 'growth', steward: 'Oracle', color: 'oklch(0.6 0.18 200)' },
  { key: 'environment', steward: 'Sage', color: 'oklch(0.55 0.2 310)' },
  { key: 'play', steward: 'Sage', color: 'oklch(0.55 0.2 310)' },
] as const

const size = 360
const cx = size / 2
const cy = size / 2
const rOuter = 168
const rInner = 56
const n = domains.length

function polar(r: number, angleDeg: number) {
  const a = (angleDeg - 90) * (Math.PI / 180)
  return { x: cx + r * Math.cos(a), y: cy + r * Math.sin(a) }
}

// each spoke is a wedge; placeholder score drives the fill radius (illustrative)
const placeholderScores = [5, 6, 6, 7, 6, 7, 7, 8, 5, 6, 5, 4]

const wedges = computed(() =>
  domains.map((d, i) => {
    const start = (360 / n) * i
    const end = (360 / n) * (i + 1)
    const score = placeholderScores[i] ?? 5
    const rScore = rInner + ((rOuter - rInner) * score) / 10
    const p1 = polar(rInner, start)
    const p2 = polar(rScore, start)
    const p3 = polar(rScore, end)
    const p4 = polar(rInner, end)
    const largeArc = end - start > 180 ? 1 : 0
    const path = [
      `M ${p1.x} ${p1.y}`,
      `L ${p2.x} ${p2.y}`,
      `A ${rScore} ${rScore} 0 ${largeArc} 1 ${p3.x} ${p3.y}`,
      `L ${p4.x} ${p4.y}`,
      `A ${rInner} ${rInner} 0 ${largeArc} 0 ${p1.x} ${p1.y}`,
      'Z',
    ].join(' ')
    const labelPos = polar(rOuter + 16, (start + end) / 2)
    return { ...d, path, labelPos, score, mid: (start + end) / 2 }
  })
)
</script>

<template>
  <section class="py-20 bg-muted/30">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="text-center mb-12">
        <h2 class="text-3xl font-bold">{{ t('lifeWheel.title') }}</h2>
        <p class="mt-3 text-lg text-muted-foreground max-w-2xl mx-auto">
          {{ t('lifeWheel.subtitle') }}
        </p>
      </div>

      <div class="grid lg:grid-cols-2 gap-10 items-center">
        <!-- Wheel SVG -->
        <div class="flex justify-center">
          <svg :viewBox="`0 0 ${size} ${size}`" class="w-full max-w-[420px] h-auto">
            <!-- guide rings -->
            <circle :cx="cx" :cy="cy" :r="rOuter" fill="none" stroke="var(--color-border)" stroke-width="1" />
            <circle :cx="cx" :cy="cy" :r="(rOuter + rInner) / 2" fill="none" stroke="var(--color-border)" stroke-width="0.5" stroke-dasharray="2 4" />
            <circle :cx="cx" :cy="cy" :r="rInner" fill="var(--color-card)" stroke="var(--color-border)" stroke-width="1" />

            <g v-for="w in wedges" :key="w.key">
              <path :d="w.path" :fill="w.color" fill-opacity="0.8" stroke="var(--color-background)" stroke-width="1.5" />
            </g>

            <!-- center -->
            <text :x="cx" :y="cy - 4" text-anchor="middle" class="fill-foreground" font-size="13" font-weight="700">12</text>
            <text :x="cx" :y="cy + 12" text-anchor="middle" class="fill-muted-foreground" font-size="7">{{ t('lifeWheel.scoreLabel') }}</text>

            <!-- labels -->
            <text
              v-for="w in wedges"
              :key="`l-${w.key}`"
              :x="w.labelPos.x"
              :y="w.labelPos.y"
              text-anchor="middle"
              dominant-baseline="middle"
              class="fill-muted-foreground"
              font-size="9"
            >{{ t(`lifeWheel.domains.${w.key}`) }}</text>
          </svg>
        </div>

        <!-- Facts + steward legend -->
        <div class="space-y-6">
          <div class="grid grid-cols-3 gap-4">
            <div class="bg-card border border-border rounded-xl p-4 text-center">
              <div class="text-2xl font-bold text-primary">12</div>
              <div class="text-xs text-muted-foreground mt-1">{{ t('lifeWheel.domains.spirit') }}…{{ t('lifeWheel.domains.play') }}</div>
            </div>
            <div class="bg-card border border-border rounded-xl p-4 text-center">
              <div class="text-2xl font-bold text-primary">1–10</div>
              <div class="text-xs text-muted-foreground mt-1">{{ t('lifeWheel.rescore') }}</div>
            </div>
            <div class="bg-card border border-border rounded-xl p-4 text-center">
              <div class="text-2xl font-bold text-primary">6+1</div>
              <div class="text-xs text-muted-foreground mt-1">{{ t('lifeWheel.stewards') }}</div>
            </div>
          </div>
          <p class="text-sm text-muted-foreground">{{ t('lifeWheel.stewardsNote') }}</p>
          <div class="grid grid-cols-2 sm:grid-cols-3 gap-2">
            <div
              v-for="d in domains"
              :key="`legend-${d.key}`"
              class="flex items-center gap-2 text-sm rounded-lg border border-border px-3 py-2"
            >
              <span class="w-3 h-3 rounded-full shrink-0" :style="{ background: d.color }" />
              <span class="truncate">{{ t(`lifeWheel.domains.${d.key}`) }}</span>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
