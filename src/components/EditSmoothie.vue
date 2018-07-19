<template>
  <v-container v-if="smoothie" style="max-width: 780px;">
  <v-card>
      <v-card-title primary-title class="mt-2">
  <h3 class="headline mt-3 indigo--text">Edit Smoothie: {{ smoothie.title }}</h3>
</v-card-title>

<div id="card-content">
  <p class="mt-2">Click <v-icon class="mx-1">add_circle</v-icon> to add, and  <v-icon class="mx-1">delete</v-icon> to delete ingredients.</p>
<v-form ref="form" v-model="valid" lazy-validation class="pt-2">


    <v-text-field
      v-model="smoothie.title"
      :counter="50"
      label="Smoothie Title"
      required
      class="mb-2"
    ></v-text-field>

    
    <div v-for="(ing, index) in smoothie.ingredients" :key="index">
   <v-text-field
     v-model="smoothie.ingredients[index]"
      :counter="50"
      label="Ingredient"
       append-icon="add_circle"
       @click:append="addIng"
       append-outer-icon="delete"
       @click:append-outer="deleteIng(ing)"
      class="mb-3"
    ></v-text-field>
    </div>

     <v-text-field
     v-model="another"
      :counter="50"
      label="Add Ingredient"
       append-icon="add_circle"
       @click:append="addIng"
      class="mb-3"
    ></v-text-field>

       <v-alert
      :value="true"
      color="error"
      icon="warning"
     
      v-if="feedback"
    >
    {{ feedback }}
    </v-alert>
    
    
       <v-btn
       color="primary"
      :disabled="!valid"
      @click="EditSmoothie"
    >
      Update Smoothie
    </v-btn>
    <v-btn @click="clear">clear</v-btn>
  </v-form>
   </div>
  </v-card>
  </v-container>
</template>


<script>
import db from "@/firebase/init";
import slugify from "slugify";
export default {
  name: "EditSmoothie",
  data() {
    return {
      valid: true,
      smoothie: null,
      another: null,
      feedback: null
    };
  },
  methods: {
    clear() {
      this.$refs.form.reset();
      this.feedback = null;
    },
    EditSmoothie() {
      if (this.smoothie.title) {
        this.feedback = null;
        // create slug
        this.smoothie.slug = slugify(this.smoothie.title, {
          replacement: "-",
          remove: /[$*_+~.()'""!\-:@]/g,
          lower: true
        });
        db
          .collection("smoothies")
          .doc(this.smoothie.id)
          .update({
            title: this.smoothie.title,
            ingredients: this.smoothie.ingredients,
            slug: this.smoothie.slug
          })
          .then(() => {
            this.$router.push({ name: "Index" });
          })
          .catch(err => {
            console.log(err);
          });
      } else {
        this.feedback = "Please Enter a Title!";
      }
    },
    addIng() {
      if (this.another) {
        this.smoothie.ingredients.push(this.another);
        this.another = null;
        this.feedback = null;
      } else {
        this.feedback = "Please Enter an Ingredient!";
      }
    },
    deleteIng(ing) {
      this.smoothie.ingredients = this.smoothie.ingredients.filter(
        ingredient => {
          return ingredient != ing;
        }
      );
    }
  },
  created() {
    let ref = db
      .collection("smoothies")
      .where("slug", "==", this.$route.params.smoothie_slug);
    ref.get().then(snapshot => {
      snapshot.forEach(doc => {
        this.smoothie = doc.data();
        this.smoothie.id = doc.id;
      });
    });
  }
};
</script>


<style scoped>
#card-content {
  padding: 0rem 2rem 2rem 2rem;
}
.v-card h3 {
  font-size: 2rem !important;
}
.v-card__title--primary {
  justify-content: center;
}
.container {
  max-width: 780px !important;
}

.container p {
  text-align: center;
}
</style>
