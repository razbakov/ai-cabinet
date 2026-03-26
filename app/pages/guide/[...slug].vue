<script setup lang="ts">
definePageMeta({
  layout: 'guide',
})

const route = useRoute()
const slugParts = computed(() => (route.params.slug as string[]) || [])
const contentPath = computed(() => slugParts.value.join('/') || 'index')

const { data: page } = await useAsyncData(
  `guide-${contentPath.value}`,
  () => queryCollection('guide').path(`/guide/${contentPath.value}`).first()
)

useHead({
  title: computed(() => page.value?.title ? `${page.value.title} — AI Cabinet Guide` : 'Guide — AI Cabinet'),
})
</script>

<template>
  <div>
    <div v-if="page" class="prose prose-invert max-w-none">
      <ContentRenderer :value="page" />
    </div>
    <div v-else class="text-center py-10">
      <Icon name="lucide:file-question" class="w-12 h-12 mx-auto mb-4 text-muted-foreground" />
      <h1 class="text-2xl font-bold mb-2">Page Not Found</h1>
      <p class="text-muted-foreground mb-6">This guide page doesn't exist yet.</p>
      <NuxtLink to="/guide" class="text-primary hover:underline">Back to Guide</NuxtLink>
    </div>
  </div>
</template>
