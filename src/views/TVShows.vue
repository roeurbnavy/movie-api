<template>
  <div class="mx-3">
    <!-- <h2 class="mt-2">Popular Movies</h2> -->
    <v-col cols="12" class="d-flex justify-center mt-5">
      <v-btn
        class="mx-2"
        fab
        dark
        small
        color="dark"
        v-on:click.prevent="previous()"
        :disabled="disabled"
      >
        <v-icon> fas fa-step-backward </v-icon>
      </v-btn>
      <v-btn
        class="mx-2"
        fab
        dark
        small
        color="dark"
        v-on:click.prevent="next()"
      >
        <v-icon dark> fas fa-step-forward </v-icon>
      </v-btn>
    </v-col>
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
          <MovieCard :movie="it" :type="`tv`" />
        </v-col>
      </v-row>
    </v-container>
    <v-col cols="12" class="d-flex justify-center mt-5">
      <v-btn
        class="mx-2"
        fab
        dark
        small
        color="dark"
        v-on:click.prevent="previous()"
        :disabled="disabled"
      >
        <v-icon> fas fa-step-backward </v-icon>
      </v-btn>
      <v-btn
        class="mx-2"
        fab
        dark
        small
        color="dark"
        v-on:click.prevent="next()"
      >
        <v-icon dark> fas fa-step-forward </v-icon>
      </v-btn>
    </v-col>
  </div>
</template>

<script>
import MovieCard from '../components/MovieCard.vue'
export default {
  components: {
    MovieCard,
  },
  data() {
    return {
      currentPage: 1,
      movies: [],
    }
  },
  watch: {
    '$route.query.page': {
      async handler(val) {
        // this.currentPage = val
        this.fetchMovies(val)
      },
      immediate: true,
      deep: true,
    },
  },
  computed: {
    disabled() {
      return this.currentPage <= 1
    },
  },
  async mounted() {
    await this.fetchMovies(this.currentPage)
  },
  methods: {
    async fetchMovies(page) {
      try {
        let res = await this.$http.get(`/tv/popular?page=${page}`)

        this.movies = res?.data?.results
      } catch (error) {
        console.log('err', error)
      }
    },
    next() {
      this.currentPage += 1
      this.$router.push({ name: 'tv', query: { page: this.currentPage } })
    },
    previous() {
      this.currentPage -= 1
      if (this.currentPage >= 1) {
        this.$router.push({ name: 'tv', query: { page: this.currentPage } })
      }
    },
  },
}
</script>

<style></style>
