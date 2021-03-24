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
    <button v-on:click="productsCreate">Add (create) a new product!</button>
    <hr />
    <h3>See current products</h3>
    <button v-on:click="productsIndex">See all products!</button>
    <div v-for="product in products" v-bind:key="product.id">
      <p>{{ product.name }}</p>
      <p><button v-on:click="productShow(product)">More Info</button></p>
      <img v-bind:src="product.image_url" />
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <h2>Here will be some info</h2>
        <p>Name: {{ currentProduct.name }}</p>
        <p>Price: {{ currentProduct.price }}</p>
        <p>Description: {{ currentProduct.description }}</p>
        <button>Close</button>
      </form>
    </dialog>
    <hr />
    <button v-on:click="productsArray">See the products array</button>
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
      message: "Using only a .vue file to access the backend ...",
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
        // console.log(this.products);
      });
    },
    productShow: function (theProduct) {
      console.log("showing product ...");
      console.log(theProduct);
      this.currentProduct = theProduct;
      document.querySelector("#product-details").showModal(); //querySelector is vanialla, or raw js - it's "outside of vue" (whatever that means) and selects a specified html element ... the #product-details was established above as an id, and it's nested text, logic etc will be shown in the modal. Also, note, that product-details is kabob case bc that's what CSS elements are written in
      this.currentProduct = theProduct;
    },
    productsArray: function () {
      console.log(this.products);
    },
  },
};
</script>
