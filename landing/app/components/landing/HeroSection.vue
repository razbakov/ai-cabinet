<template>
  <section class="relative overflow-hidden py-20 sm:py-32">
    <div class="absolute inset-0 bg-gradient-to-b from-primary/5 to-transparent" />
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative">
      <div class="text-center max-w-3xl mx-auto mb-12">
        <h1 class="text-4xl sm:text-5xl lg:text-6xl font-bold tracking-tight">
          Your AI
          <span class="text-primary">Dream Team</span>
        </h1>
        <p class="mt-6 text-lg text-muted-foreground max-w-2xl mx-auto">
          Start with a coaching session, not a config file. Discover your purpose,
          set your strategy, build your rhythm — then let 6 AI agents handle the rest.
        </p>
        <div class="mt-8 flex flex-wrap gap-4 justify-center">
          <NuxtLink
            to="/guide/getting-started"
            class="inline-flex items-center gap-2 px-6 py-3 rounded-lg bg-primary text-primary-foreground font-medium hover:opacity-90 transition-opacity"
          >
            <Icon name="lucide:rocket" class="w-5 h-5" />
            Meet the Team
          </NuxtLink>
          <NuxtLink
            to="/guide"
            class="inline-flex items-center gap-2 px-6 py-3 rounded-lg border border-border hover:bg-muted transition-colors"
          >
            <Icon name="lucide:book-open" class="w-5 h-5" />
            Read the Guide
          </NuxtLink>
        </div>
      </div>

      <div class="max-w-4xl mx-auto">
        <img
          src="/hero-team.png"
          alt="Meet the Ikigai Team — Maya, Viktor, Luna, Alex, Marco, Sage, and Kai"
          class="rounded-xl shadow-2xl border border-border"
        />
      </div>
    </div>
  </section>
</template>
