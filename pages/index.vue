<template>
  <UContainer :ui="{base: 'grid grid-cols-[repeat(auto-fill,minmax(300px,1fr))] gap-4 mx-auto py-4'}">
    <post-card v-for="post in posts" :key="post.id" :post="post">
      <UButton block>
        <NuxtLink :to="`${post.id}`" >Comments</NuxtLink>
      </UButton>
    </post-card>
  </UContainer>
  <UPagination
    v-model="page"
    :page-count="pageCount"
    :total="pageTotal"
    :ui="{
      wrapper: 'flex justify-center items-center gap-1 mb-10',
      rounded: '!rounded-full min-w-[32px] justify-center',
      default: {
        activeButton: {
          variant: 'outline'
        }
      }
    }"
  />
</template>

<script setup>

const posts = ref([]);
const page = ref(1);
const pageTotal = ref(0);
const pageCount = 30;

useFetch(async () => {
  const response = await fetch(`https://dummyjson.com/posts?limit=${pageCount}&skip=${(page.value - 1) * pageCount}`).then((res) => res.json());
  posts.value = response.posts;
  pageTotal.value = response.total;
})

</script>

