<template>
  <div :style="styleImage" class="tile">
    <p class="title">{{ product.title }}</p>
    <div v-if="discountPrice || discountPrice == 0">
      <p class="price">
        {{ discountPrice
        }}<span class="originalPrice">{{ product.price }}</span>
      </p>
    </div>
    <p v-else class="price">{{ product.price }}</p>
    <p class="description">{{ product.description }}</p>
    <div class="tab black"></div>
  </div>
</template>

<script>
export default {
  name: "Product",
  props: {
    product: {
      type: Object,
      required: true
    }
  },
  computed: {
    styleImage() {
      return "background-image: url(" + this.product.image + ")";
    },
    discountPrice: function() {
      if (this.product.discount) {
        const multiplier = parseFloat(
          (100 - this.product.discount.slice(0, -1)) / 100
        );

        return this.product.price * multiplier;
      } else {
        return null;
      }
    }
  }
};
</script>

<style lang="scss">
@import "../scss/Product.scss";
</style>
