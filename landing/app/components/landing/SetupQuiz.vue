<script setup lang="ts">
const { t } = useI18n()
const localePath = useLocalePath()

type AgentId = 'sage' | 'north' | 'torch' | 'maya'

// each option maps to an agent vote
const questions = [
  {
    id: 'q1',
    options: [
      { id: 'scattered', agent: 'north' as AgentId },
      { id: 'invisible', agent: 'torch' as AgentId },
      { id: 'empty', agent: 'sage' as AgentId },
      { id: 'chaos', agent: 'maya' as AgentId },
    ],
  },
  {
    id: 'q2',
    options: [
      { id: 'focus', agent: 'north' as AgentId },
      { id: 'audience', agent: 'torch' as AgentId },
      { id: 'meaning', agent: 'sage' as AgentId },
      { id: 'rhythm', agent: 'maya' as AgentId },
    ],
  },
  {
    id: 'q3',
    options: [
      { id: 'strategy', agent: 'north' as AgentId },
      { id: 'content', agent: 'torch' as AgentId },
      { id: 'wellbeing', agent: 'sage' as AgentId },
      { id: 'ops', agent: 'maya' as AgentId },
    ],
  },
]

const started = ref(false)
const step = ref(0)
const answers = ref<AgentId[]>([])

const total = questions.length
const finished = computed(() => started.value && answers.value.length === total)
const current = computed(() => questions[step.value])

const result = computed<AgentId>(() => {
  const tally: Record<AgentId, number> = { sage: 0, north: 0, torch: 0, maya: 0 }
  for (const a of answers.value) tally[a]++
  // Sage wins ties — every journey starts with a conversation.
  const order: AgentId[] = ['sage', 'north', 'torch', 'maya']
  let best: AgentId = 'sage'
  let bestCount = -1
  for (const a of order) {
    if (tally[a] > bestCount) {
      best = a
      bestCount = tally[a]
    }
  }
  return best
})

const agentNames: Record<AgentId, string> = {
  sage: 'Sage',
  north: 'North',
  torch: 'Torch',
  maya: 'Maya',
}

function begin() {
  started.value = true
  step.value = 0
  answers.value = []
}

function choose(agent: AgentId) {
  answers.value[step.value] = agent
  if (step.value < total - 1) step.value++
  else step.value = total // mark finished
}

function back() {
  if (step.value > 0) {
    step.value--
    answers.value = answers.value.slice(0, step.value)
  }
}

function restart() {
  started.value = true
  step.value = 0
  answers.value = []
}

const selectedAt = (i: number) => answers.value[i]
</script>

<template>
  <section class="py-20">
    <div class="max-w-3xl mx-auto px-4 sm:px-6 lg:px-8">
      <div class="text-center mb-10">
        <h2 class="text-3xl font-bold">{{ t('setupQuiz.title') }}</h2>
        <p class="mt-3 text-lg text-muted-foreground">{{ t('setupQuiz.subtitle') }}</p>
      </div>

      <div class="bg-card border border-border rounded-2xl p-6 sm:p-10">
        <!-- Intro -->
        <div v-if="!started" class="text-center">
          <Icon name="lucide:compass" class="w-12 h-12 text-primary mx-auto mb-6" />
          <button
            class="inline-flex items-center gap-2 px-6 py-3 rounded-lg bg-primary text-primary-foreground font-medium hover:opacity-90 transition-opacity"
            @click="begin"
          >
            <Icon name="lucide:play" class="w-4 h-4" />
            {{ t('setupQuiz.start') }}
          </button>
        </div>

        <!-- Questions -->
        <div v-else-if="!finished">
          <div class="flex items-center justify-between mb-6">
            <span class="text-xs font-mono text-muted-foreground">
              {{ t('setupQuiz.progress', { current: step + 1, total }) }}
            </span>
            <div class="flex gap-1.5">
              <span
                v-for="i in total"
                :key="i"
                class="h-1.5 w-8 rounded-full transition-colors"
                :class="i - 1 <= step ? 'bg-primary' : 'bg-muted'"
              />
            </div>
          </div>

          <h3 class="text-xl font-semibold mb-6">
            {{ t(`setupQuiz.questions.${current.id}.prompt`) }}
          </h3>

          <div class="grid gap-3">
            <button
              v-for="opt in current.options"
              :key="opt.id"
              class="text-left px-5 py-4 rounded-xl border transition-all hover:border-primary hover:bg-primary/5"
              :class="selectedAt(step) === opt.agent ? 'border-primary bg-primary/5' : 'border-border'"
              @click="choose(opt.agent)"
            >
              {{ t(`setupQuiz.questions.${current.id}.options.${opt.id}`) }}
            </button>
          </div>

          <div class="mt-6 flex justify-between">
            <button
              v-if="step > 0"
              class="inline-flex items-center gap-1 text-sm text-muted-foreground hover:text-foreground"
              @click="back"
            >
              <Icon name="lucide:arrow-left" class="w-4 h-4" /> {{ t('setupQuiz.back') }}
            </button>
            <span v-else />
          </div>
        </div>

        <!-- Result -->
        <div v-else class="text-center">
          <div class="w-20 h-20 mx-auto rounded-full bg-primary/10 flex items-center justify-center mb-5">
            <Icon name="lucide:sparkles" class="w-9 h-9 text-primary" />
          </div>
          <h3 class="text-2xl font-bold mb-3">
            {{ t('setupQuiz.result.title', { agent: agentNames[result] }) }}
          </h3>
          <p class="text-muted-foreground max-w-md mx-auto mb-3">
            {{ t(`setupQuiz.result.agents.${result}`) }}
          </p>
          <p class="text-sm text-muted-foreground/80 max-w-md mx-auto mb-8">
            {{ t('setupQuiz.result.sageNote') }}
          </p>
          <div class="flex flex-wrap gap-3 justify-center">
            <NuxtLink
              :to="localePath('/guide/getting-started')"
              class="inline-flex items-center gap-2 px-6 py-3 rounded-lg bg-primary text-primary-foreground font-medium hover:opacity-90 transition-opacity"
            >
              <Icon name="lucide:notebook-pen" class="w-4 h-4" />
              {{ t('setupQuiz.result.cta', { agent: agentNames[result] }) }}
            </NuxtLink>
            <button
              class="inline-flex items-center gap-2 px-6 py-3 rounded-lg border border-border hover:bg-muted transition-colors"
              @click="restart"
            >
              <Icon name="lucide:rotate-ccw" class="w-4 h-4" />
              {{ t('setupQuiz.restart') }}
            </button>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
