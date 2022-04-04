<template>
  <div class="mx-3">
    <h2 class="mt-2">TV Show</h2>
    <v-container fluid>
      <v-row>
        <v-col
          cols="12"
          lg="2"
          md="2"
          sm="4"
          xs="3"
          v-for="it in movies"
          :key="it.id"
        >
          <MovieCard :movie="it" :genres="genres" :type="`tv`" />
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import MovieCard from './MovieCard.vue'
export default {
  components: {
    MovieCard,
  },
  data() {
    return {
      movies: [],
      genres: [],
    }
  },

  async mounted() {
    await this.fetchGenres()
    try {
      let res = await this.$http.get('/tv/popular')

      this.movies = res?.data?.results
    } catch (error) {
      console.log('err', error)
    }
  },
  methods: {
    async fetchGenres() {
      try {
        let res = await this.$http.get('/genre/movie/list')
        this.genres = res?.data?.genres
      } catch (error) {
        console.log('err', error)
      }
    },
  },
}
</script>

<style></style>
