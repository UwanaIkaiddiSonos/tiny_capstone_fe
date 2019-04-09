<template>
  <div class="root">
    i am the show page
    <p>{{ product.name }}</p>
    <p>{{ product.price }}</p>
    <img v-bind:src="product.image_url">
    <p>{{ product.description }}</p>
    <router-link v-bind:to="'/products/' + product.id + '/edit'">Edit the product</router-link>
  </div>
</template>




<script>
import axios from "axios"
export default {
  data: function() {
    return {
      product: {
        name: "tortoleeney",
        price: "brian",
        image_url: "https://tastesbetterfromscratch.com/wp-content/uploads/2015/07/Roasted_Red_Pepper_Tortellini1.jpg",
        description: "torts and lini",
      }
    };
  },
  created: function() {
    axios.get("/api/products/" + this.$route.params.id).then(response => {
      console.log(response.data)
      this.product = response.data;
    })
    // console.log(this.$route.params.id)
  },
  methods: {}
};
</script>