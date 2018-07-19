<template>
  <v-container fluid>
     <v-card v-for="smoothie in smoothies" :key="smoothie.id">
        <v-card-title primary-title class="mt-2">
          <div>
            <h3 class="headline mb-0 indigo--text">{{ smoothie.title }}</h3>
            <div class="mt-4" id="chips">
              <v-chip v-for="(ing, index) in smoothie.ingredients" :key="index">{{ ing }}</v-chip>
            </div>
          </div>
        </v-card-title>

        <v-card-actions class="mb-4" id="actions">

               <router-link :to="{name:'EditSmoothie', params: {smoothie_slug: smoothie.slug}}">
               <v-btn color="primary" fab small dark class="mx-3">
              <v-icon>edit</v-icon>
            </v-btn>
             </router-link>
             
              <v-btn color="primary" fab small dark class="mx-3" @click="deleteSmoothie(smoothie.id)">
              <v-icon>delete</v-icon>
            </v-btn>
        </v-card-actions>

      </v-card>

  </v-container>
</template>


<script>
import db from "@/firebase/init";
export default {
  name: "Index",
  data() {
    return {
      smoothies: []
    };
  },
  methods: {
    deleteSmoothie(id) {
      db
        .collection("smoothies")
        .doc(id)
        .delete()
        .then(() => {
          this.smoothies = this.smoothies.filter(smoothie => {
            return smoothie.id != id;
          });
        });
    }
  },
  created() {
    // fetch data from firestore
    db
      .collection("smoothies")
      .get()
      .then(snapshot => {
        snapshot.forEach(doc => {
          let smoothie = doc.data();
          smoothie.id = doc.id;
          this.smoothies.push(smoothie);
        });
      });
  }
};
</script>

<style scoped>
.container {
  display: grid;
  grid-gap: 1.5rem;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
}
.v-card h3 {
  text-align: center;
  font-size: 2rem !important;
}

.v-card {
  display: grid;
  justify-content: center;
}

.v-card__actions {
  justify-content: center;
}
</style>

