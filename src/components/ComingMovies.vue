<template>
  <div class="mx-3">
    <h2 class="mt-2">Coming Soon</h2>
    <!-- <carousel-3d
      :controls-visible="true"
      :clickable="false"
      :key="upcomingMovies.length"
      :listData="upcomingMovies"
      :height="500"
    >
      <slide v-for="(it, i) in this.upcomingMovies" :index="i" :key="i">
        <figure>
          <img :src="'https://image.tmdb.org/t/p/w500/' + it.poster_path" />
          <figcaption>
            <v-btn :to="`/movie/${it.id}`" text color="white">
              {{ it.title }}
            </v-btn>
          </figcaption>
        </figure>
      </slide>
    </carousel-3d> -->
    <v-container fluid>
      <v-row>
        <v-col
          cols="12"
          lg="2"
          md="2"
          sm="4"
          xs="3"
          v-for="it in upcomingMovies"
          :key="it.id"
        >
          <MovieCard :movie="it" />
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
// import { Carousel3d, Slide } from 'vue-carousel-3d'
import MovieCard from './MovieCard'
export default {
  components: {
    // Carousel3d,
    // Slide,
    MovieCard,
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
          'https://api.themoviedb.org/3/movie/upcoming'
        )

        this.upcomingMovies = res?.data?.results
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
