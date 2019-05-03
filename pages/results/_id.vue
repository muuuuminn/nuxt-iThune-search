<template>
  <div>
    <h1>Results for {{ $route.params.id }}</h1>
    <div v-if="albumExists">
      <div v-for="(album, index) in albums">
        <Card
          :title="album.collectionCensoredName"
          :image="album.artworkUrl100"
          :artistName="album.artistName"
          :url="album.artistViewUrl"
          :color="picker(index)"
        />
      </div>
    </div>
    <div v-else>
      <h1>Could not find album</h1>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Card from '@/components/Card.vue';

export default {
  middleware: 'search',
  components: {
    Card
  },
  asyncData({params}) {
    return axios.get(`https://itunes.apple.com/search?term=${params.id}&entity=album`)
    .then((res) => {
      return {albums: res.data.results}
    });
  },
  methods: {
    picker(index) {
      return index % 2 == 0 ? 'red' : 'blue';
    }
  },
  computed: {
    albumExists() {
      return this.albums.length > 0;
    }
  },
}
</script>
