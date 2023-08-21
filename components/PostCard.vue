<template>
  <UCard :ui="{base: 'flex flex-col justify-between', body: {base: 'flex flex-col gap-2', padding: 'px-2 py-3 sm:p-4'}}">
    <template #header>
      <UBadge color="primary" variant="outline">#{{ post.id }} </UBadge>
      <h2 class="inline pl-4 font-semibold text-xl text-gray-900 dark:text-white leading-tight">{{ post.title }}</h2>
    </template>
    <p>{{ post.body }}</p>
    <em>user #{{ post.userId }}</em>
    <UContainer :ui="{base: 'flex gap-2 px-0 sm:px-0 lg:px-0'}">
      <UBadge v-for="tag in post.tags" :key="tag" color="gray">
        {{ tag }}
      </UBadge>
    </UContainer>
    <p>likes: {{ post.reactions }}</p>
    <template #footer>
      <UContainer :ui="{base: 'flex flex-col gap-2'}">
        <UCard v-for="comment in comments" :key="comment.id">
          <strong>user #{{ comment.id }}:</strong>
          <p>{{ comment.body }}</p>
        </UCard>
      </UContainer>
    </template>
  </UCard>
</template>

<script setup lang="ts">

type Post = {
  id: number,
  title: string,
  body: string,
  tags: string[],
  userId: number,
  reactions: number,
}

type Comment = {
  id: number,
  body: string,
  postId: number,
  user: {
    id: number,
    username: string,
  }
}

defineProps ({
  post: {
    type: Object as () => Post,
    required: true
  },
  comments: {
    type: Object as () => Comment[],
    required: true
  }
})

</script>
