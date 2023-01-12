<template>
  <v-footer dark padless>
    <v-card flat tile class="white--text text-center">
      <v-card-text>
        <v-btn v-for="icon in icons" :key="icon" class="mx-4 white--text" icon>
          <v-icon size="24px">{{ icon }}</v-icon>
        </v-btn>
      </v-card-text>
      <v-card-text class="white--text pt-0">
        MOVIE API is a Free Movies streaming site with zero ads. We let you watch
        movies online without having to register or paying, with over 10000
        movies and TV-Series. You can also Download full movies from Ah tery and
        watch it later if you want.
      </v-card-text>

      <v-divider></v-divider>

      <v-card-text class="white--text">
        {{ new Date().getFullYear() }} - <strong>AH TERY </strong>
      </v-card-text>
    </v-card>
  </v-footer>
</template>

<script>
export default {
  data() {
    return {
      icons: ['mdi-facebook', 'mdi-twitter', 'mdi-linkedin', 'mdi-instagram'],
    }
  },
}
</script>

<style></style>
