<template>
  <div class="yt-list">
    <Loading v-if="isLoading"  />
    <Items 
      v-else
      :items="fitlerItems" 
      :filters="filters" 
      :lastPage="lastPage" 
      :isLoading="isLoading" 
      @set-filters="filtersChanged($event)"
      @set-loadmore="loadMore($event)"
    />

  </div>
</template>

<script>
  import Items from "~/components/Items";
  import Loading from "~/components/Loading";
  import { YOUTUBE_API_URL, YOUTUBE_TOKEN_KEY } from "~/utils/constants"
  import axios from "axios";

  export default {
    components: { Items },
    data() {
      return {
        items: [],
        fitlerItems: [],
        filters: {
          s: '',
          page: 1,
        },
        lastPage: 0,
        perPage: 9,
        pageToken: "",
        isLoading: false,
      }
    },

    async created() {

      this.isLoading = true;
      const { data } = await axios.get(`${YOUTUBE_API_URL}/playlistItems?key=${YOUTUBE_TOKEN_KEY}&playlistId=PLbpi6ZahtOH4MCLZywkbRXiHQ4T40RrAD&part=snippet&maxResults=${this.perPage}`);
      this.items = data.items;
      this.fitlerItems = data.items.slice(0, this.filters.page * this.perPage);
      this.lastPage = Math.ceil(data.pageInfo.totalResults / this.perPage);
      this.pageToken = data?.nextPageToken || "";
      this.isLoading = false;
    },

    methods: {
      filtersChanged(f) {
        this.filters.s = f.s;
        this.filters.page = f.page;

        const items = this.items.filter(item => item.snippet.title.toLowerCase().indexOf(this.filters.s.toLowerCase()) >= 0);

        this.lastPage = Math.ceil(items.length / this.perPage);
        this.fitlerItems = items.slice(0, this.filters.page * this.perPage);
      },

      async loadMore(f) {
        if (this.pageToken !== "") {
          this.filters.page = f.page;
          this.filters.s = f.s;

          const { data } = await axios.get(`${YOUTUBE_API_URL}/playlistItems?key=${YOUTUBE_TOKEN_KEY}&playlistId=PLbpi6ZahtOH4MCLZywkbRXiHQ4T40RrAD&part=snippet&maxResults=${this.perPage}&pageToken=${this.pageToken}`);

          this.items = this.items.concat(data.items);
          this.fitlerItems = this.fitlerItems.concat(data.items.slice(0, this.filters.page * this.perPage));

          this.lastPage = Math.ceil(data.pageInfo.totalResults / this.perPage);
          this.pageToken = data?.nextPageToken || "";
        }
        
      }
    }
  }
</script>
