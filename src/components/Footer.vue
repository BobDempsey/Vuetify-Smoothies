<template>
  <v-footer
    dark
    height="auto"
    class="mt-5"
  >
    <v-card
      flat
      tile
      class="white--text text-xs-center primary darken-2 w100"
    >
  

      <v-card-text class="white--text">
        This CRUD web application is made with Vue JS, Vuetify, & Cloud Firestore.
      </v-card-text>

      <v-divider></v-divider>

      <v-card-text class="white--text">
        &copy; 2018 | <strong><a href="https://bobd.me/" target="_blank">BobD.me</a></strong>
      </v-card-text>
    </v-card>
  </v-footer>
</template>

<script>
export default {
  name: "Footer",
  data() {
    return {};
  }
};
</script>

<style scoped>
.v-card {
  width: 100%;
}
</style>
