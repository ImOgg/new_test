<script setup lang="ts">
const { data: posts, isPending: isLoading, isError, error } = useQuery({
  queryKey: ['posts'],
  queryFn: () => $fetch('http://localhost:3001/posts')
})
</script>

<template>
  <div>
    <h1 class="text-xl mb-4">Posts List</h1>

    <p v-if="isLoading">Loading...</p>
    <p v-else-if="isError">Error: {{ error.message }}</p>

    <ul v-else>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>
  </div>
</template>