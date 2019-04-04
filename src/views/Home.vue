<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <input type="text">
    <button v-on:click="addNewProduct">Add a new product</button>
    <div v-for="product in products">
      <p>{{ product.name }}</p>
      <p>{{ product.price }}</p>
      <p>{{ product.description }}</p>
      <p><img v-bind:src="product.image_url"></p>
      <hr>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: []
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    })
  },
  methods: {
    addNewProduct: function() {
      console.log('add new product')
      var params = {
        name: "painting",
        price: 40,
        description: "by VanGogh",
        image_url: "#"
      }
    }
  }
};
</script>