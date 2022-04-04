<template>
  <div>
    <h2 class="mt-6">You may also like</h2>

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
          <MovieCard :movie="it" :type="type" />
        </v-col>
      </v-row>
    </v-container>
  </div>
</template>

<script>
import MovieCard from './MovieCard.vue'
export default {
  props: {
    movieId: {
      type: Number,
      // required: true,
      default: null,
    },
  },
  components: {
    MovieCard,
  },
  data() {
    return {
      movies: [],
      type: 'movie',
    }
  },
  watch: {
    movieId: {
      async handler(val) {
        if (val) {
          try {
            // Query type : movie,tv
            this.type = this.$route?.query?.type
            let res = await this.$http.get(
              `/${this.type}/${this.movieId}/similar`
            )

            this.movies = res?.data?.results.slice(0, 12)
          } catch (error) {
            console.log('err', error)
          }
        }
      },
      immediate: true,
    },
  },
}
</script>

<style></style>
