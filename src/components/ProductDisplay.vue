<template>
  <div :class="genderClass">
    <div class="background"></div>
    <AppLoader v-if="!showProduct" />
    <div v-else>
      <div v-if="dataProduct">
        <div class="container product-container">
          <div class="img-container">
            <img :src="dataProduct.image" alt="Product Image" />
          </div>
          <div class="product-data">
            <h3 class="title">{{ dataProduct.title }}</h3>
            <div class="rating-container">
              <p class="category">{{ dataProduct.category }}</p>
              <div class="rating-point">
                <p class="rating">{{ dataProduct.rating.rate }}/5</p>
                <div class="rating-dot-container">
                  <div
                    class="rating-dot"
                    :class="{ active: j <= Math.round(dataProduct.rating.rate) }"
                    v-for="j in 5"
                    :key="j"
                  ></div>
                </div>
              </div>
            </div>
            <p class="description">{{ dataProduct.description }}</p>
            <div class="product-footer">
              <h4 class="price">${{ dataProduct.price }}</h4>
              <div class="btn-container">
                <button class="buy-btn btn">Buy Now</button>
                <button
                  class="next-btn btn men-border-color men-color"
                  @click="nextProduct"
                >
                  Next Product
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div v-else>
        <div class="container unavailable-container ">
          <p>This product is unavailable to show</p>
          <button class="btn" @click="nextProduct">Next Product</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AppLoader from "./AppLoader.vue";

export default {
  components: {
    AppLoader,
  },
  data() {
    return {
      index: 1,
      dataProduct: null,
      showProduct: false,
    };
  },

  computed: {
    genderClass() {
      if (this.dataProduct) {
        return this.dataProduct.category.includes("women")
          ? "women pattern"
          : "men pattern";
      }
      return "";
    },
  },

  methods: {
    async fetchData() {
      this.showProduct = false;
      try {
        const response = await fetch(
          `https://fakestoreapi.com/products/${this.index}`
        );
        const data = await response.json();
        data.category.includes("clothing")
          ? (this.dataProduct = data)
          : (this.dataProduct = null);
        this.showProduct = true;
      } catch (error) {
        console.error(`Error fetching data: ${error}`);
      }
    },
    nextProduct() {
      this.index = (this.index === 20) ? 1 : this.index + 1;
      this.fetchData();
    },
  },
  
  beforeMount() {
    this.fetchData();
  },
};
</script>

<style>

</style>
