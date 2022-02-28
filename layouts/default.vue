<template>
  <v-app>
    <v-app-bar app elevation="0">
      <v-container>
        <div class="d-flex align-center justify-space-between">
          <v-app-bar-title class="font-weight-bold"
            >Nuxt & Vuetify Project</v-app-bar-title
          >
          <v-btn x-large icon @click="toggleTheme">
            <v-icon>mdi-{{ dark ? "weather-sunny" : "weather-night" }}</v-icon>
          </v-btn>
        </div>
      </v-container>
    </v-app-bar>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      dark: this.$vuetify.theme.dark,
    };
  },
  methods: {
    toggleTheme() {
      this.$vuetify.theme.dark = !this.$vuetify.theme.dark;
      this.dark = this.$vuetify.theme.dark;
    },
  },
};
</script>

<style></style>
