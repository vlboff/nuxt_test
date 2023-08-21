<template>
  <UCard :ui="{base: 'flex flex-col justify-between', body: {base: 'flex flex-col gap-2', padding: 'px-2 py-3 sm:p-4'}}">
    <template #header>
      <UBadge color="primary" variant="outline">#{{ post.id }} </UBadge>
      <h2 class="inline pl-4 font-semibold text-xl text-gray-900 dark:text-white leading-tight">{{ post.title }}</h2>
    </template>
    <slot name='body'></slot>
    <UContainer :ui="{base: 'flex gap-2 px-0 sm:px-0 lg:px-0'}">
      <UBadge v-for="tag in post.tags" :key="tag" color="gray">
        {{ tag }}
      </UBadge>
    </UContainer>
    <p>likes: {{ post.reactions }}</p>
    <template #footer>
      <slot></slot>
    </template>
  </UCard>
</template>

<script setup lang="ts">

type Post = {
  id: number
  title: string
  body: string
  tags: string[]
  userId: number
  reactions: number
}

defineProps ({
  post: {
    type: Object as () => Post,
    required: true
  }
})

</script>
