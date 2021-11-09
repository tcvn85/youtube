<template>
  <div class="text-center mt-5">
    <h1 class="mb-3" v-if="error.statusCode === 404">Page not found</h1>
    <h1 class="mb-3" v-else>An error occurred</h1>
    <NuxtLink to="/">RETURN HOME PAGE</NuxtLink>
  </div>
</template>

<script>
  export default {
    props: ['error'],
    layout: 'error'
  }
</script>