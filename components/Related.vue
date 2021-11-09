<template>
  <div class="related">
    <h2 class="mb-4 text-2xl">RECOMMENDED</h2>
    <ul class="yt-items grid md:grid-cols-5 gap-4">
      <li v-for="item in items" :key="item.id.videoId">
        <Item 
          :link="`/detail/${item.id.videoId}`" 
          :src="item.snippet.thumbnails.high.url"
          :title="item.snippet.title"
          :date="item.snippet.publishedAt"
        />
      </li>

    </ul>
  </div>
</template>

<script>
  import Item from "~/components/Item";
  export default {
    name:"Related",
    props: {
     items: Array
    },
  }
</script>