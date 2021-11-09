<template>
  <div class="detail">
    <div class="yt-player mb-6 mt-5">
      <iframe width="100%" height="550" :src="`https://www.youtube.com/embed/${id}`" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </div>

    <Related :items="related.items" />
  </div>
</template>

<script>
  import Related from "~/components/Related";
  import axios from "axios";
  import { YOUTUBE_API_URL, YOUTUBE_TOKEN_KEY } from "~/utils/constants"

  export default {

    components: { Related },

    async asyncData ({ params }) {
      try {
        
        const { id } = params;

        const item = await axios.get(`${YOUTUBE_API_URL}/videos?part=snippet&id=${id}&key=${YOUTUBE_TOKEN_KEY}&type=video`).then(res => res.data);
        
        const related = await axios.get(`${YOUTUBE_API_URL}/search?part=snippet&relatedToVideoId=${id}&type=video&key=${YOUTUBE_TOKEN_KEY}`).then(res => res.data);
      
        return {
          item: item,
          related: related,
          id: id
        }
      } catch(error) {
        console.log(error);        
      }
    },
  }
</script>
