<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h3>Make a product</h3>
    <p>
      name:
      <input type="text" v-model="newProductName" />
    </p>
    <p>
      price:
      <input type="text" v-model="newProductPrice" />
    </p>
    <p>
      description:
      <input type="text" v-model="newProductDescription" />
    </p>
    <p>
      image url:
      <input type="text" v-model="newProductImageUrl" />
    </p>
    <button v-on:click="productsCreate">Create the product</button>
    <hr />
    <button v-on:click="productsIndex">Get the Index</button>
    <div v-for="product in products" v-bind:key="product.id">
      <p>{{ product.name }}</p>
      <p><button v-on:click="productShow(product)">More Info</button></p>
      <img v-bind:src="product.image_url" />
    </div>
    <dialog id="product-details">
      <h2>Here will be some info</h2>
      <p>Title: (currentProduct.title)</p>
      <p>Price: (currentProduct.price)</p>
      <p>Description: (currentProduct.description)</p>
    </dialog>
  </div>
</template>
<style>
img {
  width: 250px;
}
</style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "The brave new world of View.js! ...",
      products: [],
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: "",
      currentProduct: {},
    };
  },
  created: function () {
    // this.indexProducts();
  },
  methods: {
    productsIndex: function () {
      console.log("in products index");
      // get data from the api, then show that data to the user
      axios.get("http://localhost:3000/api/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    productsCreate: function () {
      console.log("products create...");
      // make a post request to the api
      console.log(this.newProductName);
      var params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        image_url: this.newProductImageUrl,
      };
      axios.post("http://localhost:3000/api/products", params).then((response) => {
        console.log(response.data);
        this.products.push(response.data);
        this.newProductName = "";
        this.newProductPrice = "";
        this.newProductDescription = "";
        this.newProductImageUrl = "";
      });
    },
    productShow: function (theProduct) {
      console.log("showing product ...");
      console.log(theProduct);
      document.querySelector("#product-details").showModal();
      this.currentProduct = theProduct;
    },
  },
};
</script>
