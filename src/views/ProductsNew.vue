<template>
  <div class="root">
    I am the /products/new page
    <div v-for="error in errors">
      {{ error }}
    </div>

    <form v-on:submit.prevent="makeRecipe()">
      <p>Name: <input type="text" v-model="newProductName"></p>
      <p>Price: <input type="text" v-model="newProductPrice"></p>
      <p>Description: <input type="text" v-model="newProductDescription"></p>
      <input type="submit" value="Add a new product">
      <!-- <button>Make a new recipe</button> -->
    </form>
  </div>
</template>

<script>
import axios from "axios"
export default {
  data: function() {
    return {
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      errors: []
    };
  },
  created: function() {},
  methods: {
    addProduct: function() {
      console.log('adding the new product');
      // make a request to the api to the create action
      // response = HTTP.get("/api/products")
      // products = reponse.parse
      var params = {
        input_title: this.newProductName,
        input_ingredients: this.newProductPrice,
        input_directions: this.newProductDescription
      }
      axios.post("/api/products", params).then(response => {
        console.log('things are going well')
        console.log(response);
        this.$router.push("/")
      }).catch(error => {
        console.log('things are going poorly')
        console.log(error.response.data.errors)
        this.errors = error.response.data.errors;
      });
    }
  }
};
</script>
