<template>
  <div class="yt-list">
    <div class="search-box">
      <form v-on:submit.prevent="onSearch" type="submit">
        <div class="relative mt-4 mb-5">
          <button class="text-gray-900 absolute top-0 left-0 btn" >
            <svg width="30" height="30" aria-hidden="true" focusable="false" data-prefix="far" data-icon="search" role="img" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512" class="svg-inline--fa fa-search fa-w-16 fa-2x mx-auto"><path fill="currentColor" d="M508.5 468.9L387.1 347.5c-2.3-2.3-5.3-3.5-8.5-3.5h-13.2c31.5-36.5 50.6-84 50.6-136C416 93.1 322.9 0 208 0S0 93.1 0 208s93.1 208 208 208c52 0 99.5-19.1 136-50.6v13.2c0 3.2 1.3 6.2 3.5 8.5l121.4 121.4c4.7 4.7 12.3 4.7 17 0l22.6-22.6c4.7-4.7 4.7-12.3 0-17zM208 368c-88.4 0-160-71.6-160-160S119.6 48 208 48s160 71.6 160 160-71.6 160-160 160z" class=""></path></svg>
          </button>
          <input 
            type="text" 
            placeholder="Search Videos" 
            class="form-input text-black px-4 py-3 w-full rounded md:rounded-lg outline-none"
            @keyup="search($event.target.value)"
          />
        </div>
      </form>
    </div>

    <ul class="yt-items grid md:grid-cols-3 gap-6" v-if="items.length > 0">
      <li v-for="item in items" :key="item.id" class="mb-5">
        <Item 
          :link="`/detail/${item.snippet.resourceId.videoId}`" 
          :src="item.snippet.thumbnails.high.url"
          :title="item.snippet.title" 
          :date="item.snippet.publishedAt"
        />
      </li>
    </ul>
    <div v-else>No result found!</div>

    <div v-if="filters.page < lastPage" class="text-center bg-gray-400 text-white mb-5">
      <button class="block w-full pt-3 pb-3" @click="loadMore">Load More</button>
    </div>

  </div>
</template>

<script>
  import Item from "~/components/Item";
  export default {
    name: "Items",
    props: ['items', 'filters', 'lastPage', 'isLoading'],
    methods: {
      search(s) {
        this.$emit('set-filters', {
          ...this.filters,
          s,
          page: 1
        });
      },  
      loadMore() {
        this.$emit('set-loadmore', {
          ...this.filters,
          page: this.filters.page + 1
        });
      },
    }
  }
</script>

<style scope>
  .btn{
    height: 48px;
    width: 48px;
    text-align: center;
    line-height: 48px;
  }
  .form-input{
    padding-left: 55px;
  }

</style>
