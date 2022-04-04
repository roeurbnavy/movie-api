<template>
  <div>
    <!-- <h2 class="mx-3 grey--text">Upcoming Movies</h2> -->
    <carousel-3d
      :controls-visible="true"
      :clickable="false"
      :key="upcomingMovies.length"
      :listData="upcomingMovies"
      :height="500"
    >
      <slide v-for="(it, i) in this.upcomingMovies" :index="i" :key="i">
        <figure
          :style="`background-image: url(${
            'https://image.tmdb.org/t/p/w500/' + it.poster_path
          }`"
        >
          <img :src="'https://image.tmdb.org/t/p/w500/' + it.poster_path" />
          <figcaption>
            <v-btn :to="`/movie/${it.id}`" text color="white">
              {{ it.title }}
            </v-btn>
          </figcaption>
        </figure>
      </slide>
    </carousel-3d>
    <!-- <v-carousel
      cycle
      height="400"
      hide-delimiter-background
      show-arrows-on-hover
    >
      <v-carousel-item
        v-for="(it, i) in upcomingMovies"
        :key="i"
        :src="'https://image.tmdb.org/t/p/w500/' + it.poster_path"
      >
        <v-row class="fill-height" align="center" justify="center">
          <img :src="'https://image.tmdb.org/t/p/w500/' + it.poster_path" />
        </v-row>
      </v-carousel-item>
    </v-carousel> -->
  </div>
</template>

<script>
//Reference : https://github.com/Wlada/vue-carousel-3d/issues/26
import { Carousel3d, Slide } from 'vue-carousel-3d'
export default {
  components: {
    Carousel3d,
    Slide,
  },
  data() {
    return {
      upcomingMovies: [],
    }
  },
  mounted() {
    this.fetchComingMovies()
  },
  methods: {
    async fetchComingMovies() {
      try {
        let res = await this.$http.get(
          'https://api.themoviedb.org/3/movie/now_playing'
        )

        this.upcomingMovies = res.data.results.slice(1, 6)
      } catch (error) {
        console.log('error', error)
      }
    },
  },
}
</script>

<style>
.carousel-3d-container figure {
  margin: 0;
}
.carousel-3d-container figcaption {
  position: absolute;
  background-color: rgba(0, 0, 0, 0.5);
  color: white;
  bottom: 0;
  padding: 15px;
  font-size: 12px;
  min-width: 100%;
  box-sizing: border-box;
}

.next span,
.prev span {
  color: red;
}
</style>
