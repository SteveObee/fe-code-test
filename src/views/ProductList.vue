<template>
  <div>
    <label for="selector">
      Filter:
      <select v-model="select" id="selector">
        <option value="All">All</option>
        <option value="Purchased">Purchased</option>
        <option value="Unpurchased">Unpurchased</option>
        <option value="One time purchases">One time purchases</option>
        <option value="Subscriptions">Subscriptions</option>
      </select>
    </label>
    <h1 class="py-2">SELECTED FILTER: {{ selectedFilter }}</h1>
    <div class="grid-container" v-if="products.length">
      <Product
        v-for="(product, idx) in sortedProducts(products)"
        :key="idx"
        :product="product"
      />
    </div>
  </div>
</template>

<style>
@import "../scss/ProductList.scss";
</style>

<script>
const productItems = require("@/assets/products.json");
import Product from "../components/Product.vue";

export default {
  components: {
    Product
  },
  name: "ProductList",
  computed: {
    products() {
      let products = [];
      if (this.selectedFilter == "All") {
        productItems.forEach((product, idx) => {
          products.push(productItems[idx]);
        });

        return products;
      } else if (this.selectedFilter == "Purchased") {
        productItems.forEach((product, idx) => {
          if (product.purchased) {
            products.push(productItems[idx]);
          }
        });
        return products;
      } else if (this.selectedFilter == "Unpurchased") {
        productItems.forEach((product, idx) => {
          if (!product.purchased) {
            products.push(productItems[idx]);
          }
        });
        return products;
      } else if (this.selectedFilter == "One time purchases") {
        productItems.forEach((product, idx) => {
          if (product.type == "onetime") {
            products.push(productItems[idx]);
          }
        });
        return products;
      } else if (this.selectedFilter == "Subscriptions") {
        productItems.forEach((product, idx) => {
          if (product.cycle) {
            products.push(productItems[idx]);
          }
        });
        return products;
      }

      return "Product";
    }
  },
  methods: {
    sortedProducts: function(products) {
      return products.sort((a, b) => (a.order > b.order ? 1 : -1));
    }
  },
  data() {
    return {
      select: "All",
      selectedFilter: "All"
    };
  },
  watch: {
    select: function(newVal) {
      this.selectedFilter = newVal;
    }
  }
};
</script>
