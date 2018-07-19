<template>
  
<div class="navbar mb-4">
  <v-toolbar dark color="primary">
    <router-link :to="{name: 'Index'}" style="color: #fff;
  text-decoration: none;">
    <v-toolbar-title class="white--text">Vuetify Smoothies - Bob Dempsey</v-toolbar-title>
    </router-link>
            <v-fab-transition> 
              <router-link :to="{name: 'AddSmoothie'}">
              <v-btn
                color="pink"
                fab
                dark
                medium
                absolute
                bottom
                right
              >
              
                <v-icon>add</v-icon>
            
              </v-btn>
                </router-link>
            </v-fab-transition>
  </v-toolbar>
</div>
  
</template>

<script>
export default {
  name: "Navbar",
  data() {
    return {};
  }
};
</script>

<style scoped>
@media (max-width: 600px) {
  .v-toolbar__title {
    font-size: 1.25rem;
  }
}
</style>


