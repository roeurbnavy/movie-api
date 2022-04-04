<template>
  <v-app>
    <NavBar />
    <v-main>
      <router-view />
    </v-main>

    <Footer />
  </v-app>
</template>
<script>
import NavBar from '/src/components/Navigation.vue'
import Footer from '/src/components/Footer'
export default {
  name: 'App',
  components: {
    NavBar,
    Footer,
  },
  data() {
    return {}
  },
}
</script>
