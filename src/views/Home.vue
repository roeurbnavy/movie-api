<template>
  <div>
    <NowPlaying />
    <TrendingMovies />
    <PopularMovies />
    <PopularTVShow />
    <ComingMovies />
  </div>
</template>

<script>
// @ is an alias to /src
import TrendingMovies from '../components/TrendingMovies.vue'
import NowPlaying from '../components/NowPlaying.vue'
import PopularMovies from '../components/PopularMovies.vue'
import PopularTVShow from '../components/PopularTVShow.vue'
import ComingMovies from '../components/ComingMovies'
export default {
  name: 'Home',
  components: {
    NowPlaying,
    TrendingMovies,
    PopularMovies,
    PopularTVShow,
    ComingMovies,
  },
}
</script>
