<template>
  <v-hover v-slot="{ hover }" open-delay="200">
    <v-card :elevation="hover ? 16 : 2" :class="{ 'on-hover': hover }">
      <router-link :to="`/movie/${movie.id}?type=${type}`">
        <v-img :src="posterPath"></v-img>
      </router-link>

      <v-card-title class="subtitle-2">
        {{ movie.title || movie.name }}
      </v-card-title>

      <!-- <v-card-text>
        <v-row align="center" class="mx-0">
          <div class="grey--text">
            {{ movie.release_date }}
          </div>
          <div class="ml-4">
            <v-rating
              :value="movie.vote_average / 2"
              color="amber"
              dense
              half-increments
              readonly
              size="14"
            >
            </v-rating>
            <div class="grey--text ml-4">{{ movie.vote_average * 10 }} %</div>
          </div>
        </v-row>
      </v-card-text> -->
      <v-card-text>
        <v-row align="center" class="mx-0">
          <v-rating
            :value="movie.vote_average / 2"
            color="amber"
            dense
            half-increments
            readonly
            size="14"
          >
          </v-rating>
          <div class="grey--text ml-2">{{ movie.vote_average * 10 }} %</div>
        </v-row>
        <div class="grey--text mt-3">
          {{ movie.release_date }}
        </div>
        <!-- <div class="my-4 subtitle-2">
          <span v-for="(it, i) in movie.genre_ids" :key="it">
            {{ genreTypeName(it, i) }}
          </span>
        </div> -->
      </v-card-text>
    </v-card>
  </v-hover>
</template>

<script>
export default {
  props: {
    movie: {
      type: Object,
      required: true,
    },
    genres: {
      type: Array,
      optional: true,
      default() {
        return []
      },
      // required: true,
    },
    type: {
      type: String,
      default: () => {
        return 'movie'
      },
    },
  },
  computed: {
    posterPath() {
      return `https://image.tmdb.org/t/p/w500${this.movie.poster_path}`
    },
  },
  methods: {
    genreTypeName(genraId, index) {
      for (let it of this.genres) {
        if (it.id == genraId) {
          if (this.movie.genre_ids.length - 1 == index) {
            return it.name
          } else {
            return it.name + ','
          }
        }
      }
    },
  },
}
</script>

<style></style>
