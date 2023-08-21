<template>
  <UContainer :ui="{base: 'mx-auto py-4'}">
    <UButton @click="goBack">Go back</UButton>
    <post-card :post="post" :ui="{base: 'mt-4', body: {base: 'flex flex-col gap-2'}}">
      <template #body>
        <p>{{ post.body }}</p>
        <em>user #{{ post.userId }}</em>
      </template>
      <UContainer :ui="{base: 'flex flex-col gap-2'}">
        <UCard v-for="comment in comments" :key="comment.id">
          <strong>user #{{ comment.id }}:</strong>
          <p>{{ comment.body }}</p>
        </UCard>
      </UContainer>
    </post-card>
  </UContainer>
</template>

<script setup>
const { params } = useRoute();
const router = useRouter();
const post = ref({});
const comments = ref([]);

const goBack = () => {
  router.back();
}

useFetch(async () => {
  const commentsResponse = await fetch(`https://dummyjson.com/posts/${params.id}/comments`).then((res) => res.json()).then((res) => res.comments);
  const postResponse = await fetch(`https://dummyjson.com/posts/${params.id}`).then((res) => res.json());
  comments.value = commentsResponse;
  post.value = postResponse;
})

</script>
