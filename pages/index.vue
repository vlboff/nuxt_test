<template>
  <UContainer>
    <UTable
      :rows="posts?.posts"
      :columns="columns"
      :loading="pending"
      :ui="{
        tr: {
          base: 'hover:bg-gray-50 dark:hover:bg-gray-800/50 cursor-pointer'
        }
      }"
      @select="select"
    >
      <template #tags-data="{ row }">
        <div class='flex gap-2'>
          <UBadge v-for="tag in row.tags" :key="tag" color="gray">
            {{ tag }}
          </UBadge>
        </div>
      </template>
    </UTable>
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
  <UModal v-model="isOpen">
    <post-card :post="selectedPost" :comments="comments"></post-card>
  </UModal>
</template>

<script setup>

const columns = [{
  key: 'id',
  label: 'ID'
}, {
  key: 'title',
  label: 'Title'
}, {
  key: 'tags',
  label: 'Tags'
}, {
  key: 'reactions',
  label: 'Likes'
}];

const page = ref(1);
const pageTotal = ref(0);
const pageCount = 30;
const selectedPost = ref(null);
const comments = ref([]);
const isOpen = ref(false);

const { data: posts, pending } = await useAsyncData('posts',
  async () => {
    const response = await $fetch(`https://dummyjson.com/posts?limit=${pageCount}&skip=${(page.value - 1) * pageCount}`);
    pageTotal.value = response.total;
    return response;
  },
  {
    watch: [page],
  }
);

const select = async (row) => {
  const index = posts.value.posts.find((item) => item.id === row.id);
  if (index === -1) {
    selectedPost.value = null;
  } else {
    selectedPost.value = row;
  };

  const { data } = await useFetch(`https://dummyjson.com/posts/${selectedPost.value.id}/comments`, {
    pick: ['comments']
  })

  comments.value = data.value.comments;
  isOpen.value = true;
}
</script>

