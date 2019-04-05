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
      <button v-on:click="toggleInfo(product)">Show more info</button>
      <div v-if="product === currentProduct">
        <p>{{ product.description}}</p>
        <p>name: <input type="text" v-model="product.name"></p>
        <p>price: <input type="text" v-model="product.price"></p>
        <p>description: <input type="text" v-model="product.description"></p>
        <button v-on:click="updateProduct(product)">Update the product</button>
        <button v-on:click="deleteProduct(product)">Delete the product</button>      
      </div>
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
      newProductPrice: "" ,
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
    },
    toggleInfo: function(theProduct) {
      if (this.currentProduct === theProduct) {
        // I am seeing the info already
        this.currentProduct = {};
      } else {
        // we cannot see the info
        this.currentProduct = theProduct;
      }
      console.log('toggling info...');
    },
    updateProduct: function(theProduct) {
      console.log(theProduct)
      console.log('updating the product...');
      var params = {
        input_name: theProduct.name,
        input_price: theProduct.price,
        input_description: theProduct.description,
      };
      axios.patch("/api/products/" + theProduct.id, params).then(response => {
        console.log(response);
        theProduct = response.data;
      })
    },
    deleteProduct: function(theProduct) {
      console.log('deleting the product...');
      // make an HTTP request using axios to the destroy action of my API
      axios.delete("/api/products/" + theProduct.id).then(response => {
        console.log(response);
        // delete the product from the products array
        // find the index of that product in the array
        var index = this.products.indexOf(theProduct);
        // remove the item based on that index
        this.products.splice(index, 1);
      })
    }
  }
};
</script>