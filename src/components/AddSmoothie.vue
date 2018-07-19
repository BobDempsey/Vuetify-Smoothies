<template>
<v-container>
  <v-card>
      <v-card-title primary-title class="mt-2">
  <h3 class="headline mt-3 indigo--text">Add New Smootie</h3>
</v-card-title>
<div id="card-content">
   <p class="mt-2">Click <v-icon class="mx-1">add_circle</v-icon> to add, and  <v-icon class="mx-1">delete</v-icon> to delete ingredients.</p>
  <v-form ref="form" v-model="valid" lazy-validation class="pt-1">


    <v-text-field
      v-model="title"
      :counter="50"
      label="Smoothie Title"
      required
      class="mb-2"
      clearable
    ></v-text-field>

    
    <div v-for="(ing, index) in ingredients" :key="index" v-if="appendedInputs">
   <v-text-field
     v-model="ingredients[index]"
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
      @click="AddSmoothie"
    >
      Add Smoothie
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
  name: "AddSmoothie",
  data() {
    return {
      valid: true,
      title: null,
      another: null,
      feedback: null,
      appendedInputs: null,
      slug: null,
      ingredients: [],
      titleRules: [
        v => !!v || "Smoothie Title is Required",
        v =>
          (v && v.length <= 50) ||
          "Smoothie title must be less than 50 characters"
      ],
      ingRules: [
        v => !!v || "Ingredients are Required",
        v =>
          (v && v.length <= 50) ||
          "Smoothie ingredient must be less than 50 characters"
      ]
    };
  },
  methods: {
    clear() {
      this.$refs.form.reset();
      this.feedback = null;
      this.appendedInputs = null;
    },
    AddSmoothie() {
      if (this.title) {
        this.feedback = null;
        // create slug
        this.slug = slugify(this.title, {
          replacement: "-",
          remove: /[$*_+~.()'""!\-:@]/g,
          lower: true
        });
        db
          .collection("smoothies")
          .add({
            title: this.title,
            ingredients: this.ingredients,
            slug: this.slug
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
      console.log(this.title, this.ingredients);
    },
    addIng() {
      if (this.another) {
        this.ingredients.push(this.another);
        this.another = null;
        this.feedback = null;
        this.appendedInputs = true;
      } else {
        this.feedback = "Please Enter an Ingredient!";
      }
    },
    deleteIng(ing) {
      this.ingredients = this.ingredients.filter(ingredient => {
        return ingredient != ing;
      });
    }
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
