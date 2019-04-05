<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Name: <input type="text" v-model="newProductName"></p>
    <p>Price: <input type="text" v-model="newProductPrice"></p>
    <p>Description: <input type="text" v-model="newProductDescription"></p>
    <button v-on:click="addNewProduct()">Add a new product</button>

    <div v-for="product in products">
      <p>{{ product.name }}</p>
      <p>{{ product.price }}</p>
      <!--<p>{{ product.description }}</p>-->
      <p><img v-bind:src="product.image_url"></p>
      <div v-if="product === currentProduct">
        <p>{{ product.description}}</p>
      </div>
      <button v-on:click="currentProduct = product">Show more info</button>
      <hr>
    </div>
  </div>
</template>

<style>
  img {
    width: 80px;
  }
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Hello to Vue.js!",
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      currentProduct: {}
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      this.products = response.data;
    })
  },
   methods: {
    addNewProduct: function() {
      console.log('adding new product...');
      var params = {
        input_name: this.newProductName,
        input_price: this.newProductPrice,
        input_description: this.newProductDescription
      }
      console.log(params);
      // send the data to the API
      axios.post("/api/products", params).then(response => {
        console.log(response.data);
        this.products.push(response.data);
      });
    }
  }
};
</script>