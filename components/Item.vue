<template>
  <nuxt-link :to="link">
    <img :src="src" :alt="title" class="mb-4" />
    <h2 class="title">{{ title }}</h2>
    <p class="date text-gray-500">{{  dateFormat(date) }}</p>
  </nuxt-link>
</template>

<script>
  import moment from "moment";
  export default {
    props: ['link', 'src', 'title', 'date'],
    methods: {
      dateFormat(d) {
        return moment(new Date(d)).fromNow();
      }
    }
  }
</script>
<style>

</style>
