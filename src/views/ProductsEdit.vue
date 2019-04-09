<template>
  <div class="root">
    I am the edit page
    <div v-for="error in errors">
      {{ error }}
    </div>

    <form v-on:submit.prevent="updateProduct()">
      <p>Name: <input type="text" v-model="product.name"></p>
      <p>Price: <input type="text" v-model="product.price"></p>
      <p>Description: <input type="text" v-model="product.description"></p>
      <p>Image Url: <input type="text" v-model="product.image_url"></p>
      <input type="submit" value="Update product">
      <!-- <button>Make a new product</button> -->
    </form>
    <button v-on:click="deleteProduct()">Delete product</button>
  </div>
</template>

<script>
import axios from "axios"

export default {
  data: function() {
    return {
      product: {
        name: "",
        price: "",
        description: "",
        image_url: "",
        id: ""
      },
      errors: []
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
      console.log(response.data)
      this.product = response.data;
    })
  },
  methods: {
    updateProduct: function() {
      console.log('making the new product');
      // make a request to the api to the create action
      // response = HTTP.get("/api/products")
      // products = reponse.parse
      var params = {
        input_name: this.product.name,
        input_price: this.product.price,
        input_description: this.product.description,
        input_image_url: this.product.image_url,
      }
      console.log(params);
      axios.patch("/api/products/" + this.$route.params.id, params).then(response => {
        console.log('things are going well')
        console.log(response);
        this.$router.push("/products/" + this.$route.params.id)
      }).catch(error => {
        console.log('things are going poorly')
        console.log(error.response.data.errors)
        this.errors = error.response.data.errors;
      });
    },
    deleteProduct: function() {
      console.log('deleting the product...');
      // make an HTTP request using axios to the destroy action of my API
      axios.delete("/api/products/" + this.$route.params.id).then(response => {
        this.$router.push("/");
      })
    }
  }
};
</script>