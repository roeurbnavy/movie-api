<template>
  <div>
    <v-container>
      <v-row>
        <v-col cols="12" sm="4">
          <v-hover v-slot="{ hover }" open-delay="200">
            <v-card :elevation="hover ? 16 : 2" :class="{ 'on-hover': hover }">
              <!-- <router-link :to="`/movie/${movie.id}`"> -->
              <v-img :src="posterPath"></v-img>
              <!-- </router-link> -->
            </v-card>
          </v-hover>
        </v-col>

        <v-col cols="12" sm="8">
          <h1 class="grey--text text-dark-3 mt-5">
            {{ this.movie.title || this.movie.name }}
          </h1>
          <v-row>
            <v-col cols="12" sm="2">
              <v-rating
                :value="movie.vote_average"
                color="amber"
                dense
                half-increments
                readonly
                size="14"
              ></v-rating>
            </v-col>
            <v-col cols="12" sm="3">
              <span class="grey--text ml-n7">
                {{ movie.vote_average * 10 }} |
                {{ movie.runtime || movie.episode_run_time }} min
              </span>
            </v-col>
            <!-- <v-col cols="12" sm="7">
              <div class="subtitle-2 grey--text ml-n16">
                <span
                  v-for="(it, index) in movie.genres"
                  :key="index"
                  class="ml-1"
                >
                  {{ it.name }}
                  <span v-if="movie.genres.length - 1 != index">,</span>
                </span>
              </div>
            </v-col> -->
          </v-row>
          <div class="mt-3">
            <div>
              <span class="subtitle-2"
                >Release Date : {{ movie.release_date }}</span
              >
            </div>
          </div>
          <div class="mt-3">
            <div>
              <span class="subtitle-2">Genres :</span>
              <span
                v-for="(it, index) in movie.genres"
                :key="index"
                class="ml-1"
              >
                {{ it.name }}
                <span v-if="movie.genres.length - 1 != index">,</span>
              </span>
            </div>
          </div>
          <!-- <div class="mt-3">
            <div>
              <span class="subtitle-2">Actors :</span>
              <span
                v-for="(it, index) in movie.credits.cast"
                :key="index"
                class="ml-1"
              >
                {{ it.name }}
                <span v-if="movie.credits.cast.length - 1 != index">,</span>
              </span>
            </div>
          </div> -->

          <div class="mt-3">
            <span class="subtitle-2 text-darken-3">Overview : </span>
            <p>
              {{ this.movie.overview }}
            </p>
          </div>

          <div class="mt-5">
            <!-- <h2 class="mt-5 grey-text text-darken-3">Featured Cast</h2>
            <div
              v-for="(it, index) in movie.credits.crew"
              :key="index"
              class="mt-5"
            >
              <div v-if="index < 2" class="">
                <h3>{{ it.name }}</h3>
                <span class="grey--text">{{ it.job }}</span>
              </div>
            </div> -->

            <v-dialog v-model="dialog" persistent max-width="800px">
              <template v-slot:activator="{ on, attrs }">
                <v-btn
                  tile
                  color="white--text black"
                  v-bind="attrs"
                  v-on="on"
                  v-if="disabled"
                  @click.prevent="openYouTubeModel"
                >
                  <v-icon left>mdi-play</v-icon>Play
                </v-btn>
              </template>
              <v-card>
                <v-card-title>
                  <span class="headline">{{ this.movie.title }}</span>
                </v-card-title>

                <v-card-text>
                  <v-container>
                    <v-row>
                      <v-col cols="12" sm="">
                        <div class="iframe-container">
                          <img :src="mediaURL" v-if="!isVideo" />
                          <iframe
                            allowfullscreen
                            v-if="isVideo"
                            :src="mediaURL"
                          ></iframe>
                        </div>
                      </v-col>
                    </v-row>
                  </v-container>
                </v-card-text>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="error" text @click="closeDialog">Close</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </div>
        </v-col>
      </v-row>
      <!-- Cast -->
      <Casts :casts="movie.credits.cast" />
      <!-- SimilarMovies -->
      <SimilarMovies :movieId="movie.id" />
    </v-container>
  </div>
</template>

<script>
import Casts from './Cast'
import SimilarMovies from './SimilarMovies.vue'
export default {
  components: {
    SimilarMovies,
    Casts,
  },
  data() {
    return {
      movie: {
        credits: {
          crew: {},
        },
        images: {
          backdrops: {},
        },
      },
      isVideo: false,
      mediaURL: '',
      dialog: false,
    }
  },

  computed: {
    posterPath() {
      return 'https://image.tmdb.org/t/p/w500' + this.movie.poster_path
    },
    disabled() {
      return !(this.movie?.videos?.results?.length <= 0)
    },
  },

  watch: {
    '$route.params.id': {
      handler() {
        this.fetchMovie(this.$route.params.id)
      },
      immediate: true,
    },
  },

  async mounted() {
    await this.fetchMovie(this.$route.params.id)
  },
  methods: {
    async fetchMovie(id) {
      try {
        // Query type : movie,tv
        let type = this.$route?.query?.type
        let res = await this.$http.get(
          `/${type}/` + id + '?append_to_response=credits,person,videos,images'
        )
        this.movie = res.data
      } catch (error) {
        console.log('error', error)
      }
    },
    openYouTubeModel() {
      this.mediaURL = this.youtubeVideo()
      this.isVideo = true
    },
    youtubeVideo() {
      if (!this.movie.videos) return

      return 'https://www.youtube.com/embed/' + this.movie.videos.results[0].key
    },
    closeDialog() {
      this.dialog = false
      this.isVideo = false
    },
  },
}
</script>

<style>
.iframe-container {
  overflow: hidden;
  padding-top: 56.25%;
  position: relative;
}

.iframe-container iframe {
  border: 0;
  height: 100%;
  left: 0;
  position: absolute;
  top: 0;
  width: 100%;
}
</style>
