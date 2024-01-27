<script>
import axios from "axios";

export default {
  name: "ProductDisplay",
  data() {
    return {
      productData: null,
      productId: 1,
      maximumProduct: 0,
      isLoading: false,
      isProductAvailable: false,
      isMenClothing: false,
      isWomenClothing: false,
    };
  },
  methods: {
    loadData() {
      this.isLoading = true;
      axios
        .get(`https://fakestoreapi.com/products/${this.productId}`)
        .then((response) => {
          this.productData = response.data;
          this.setProductAvailability();
          this.isLoading = false;
        })
        .catch((error) => {
          console.log(error);
          this.isLoading = false;
        });
    },
    setProductAvailability() {
      if (this.productData.category === "men's clothing") {
        this.isProductAvailable = true;
        this.isMenClothing = true;
        this.isWomenClothing = false;
      } else if (this.productData.category === "women's clothing") {
        this.isProductAvailable = true;
        this.isMenClothing = false;
        this.isWomenClothing = true;
      } else {
        this.isProductAvailable = false;
        this.isMenClothing = false;
        this.isWomenClothing = false;
      }
    },
    fetchMaximumProduct() {
      axios
        .get("https://fakestoreapi.com/products")
        .then((response) => {
          this.maximumProduct = response.data.length;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    displayNextProduct() {
      this.productId = (this.productId % this.maximumProduct) + 1;
      this.loadData();
    },
  },
  mounted() {
    this.loadData();
    this.fetchMaximumProduct();
  },
};
</script>

<template>
  <div class="product-page">
    <div v-if="isLoading" class="card">
      <div class="product-container">
        <div class="loader-product-image"></div>
        <div class="loader-detail-product">
          <div class="loader-top-detail"></div>
          <div class="loader-bottom-detail"></div>
        </div>
      </div>
    </div>
    <div v-else class="container-page" :class="!isProductAvailable ? 'bg-grey' : isWomenClothing ? 'bg-pink' : 'bg-blue'">
      <div v-if="isProductAvailable" class="pattern">
        <img src="../assets/bg-pattern.svg" alt="pattern background" />
      </div>
      <div class="card">
        <div v-if="!isProductAvailable" class="product-unavailable-container">
          <div class="overlay">
            <img src="../assets/images/sad-face.png" alt="background-sad-face" />
          </div>
          <div class="detail-product">
            <p>This product is unavailable to show</p>
            <div class="buttons">
              <button @click="displayNextProduct" class="button-next">Next product</button>
            </div>
          </div>
        </div>
        <div v-else class="product-container">
          <div class="product-image">
            <img v-if="productData && productData.image" :src="productData.image" alt="image-clothes" />
          </div>
          <div class="detail-product">
            <div class="top-card">
              <h2 v-if="productData && productData.title" class="title" :class="isMenClothing ? 'text-navy' : 'text-purple'">
                {{ productData.title }}
              </h2>
              <div class="category">
                <p v-if="productData && productData.category">{{ productData.category }}</p>
                <div class="rating">
                  <span v-if="productData && productData.rating">{{ productData.rating.rate }}</span>
                  <div class="circles-rate">
                    <span class="circle" :class="productData.rating.rate < 0.5 ? (isMenClothing ? 'bgrd-white-navy' : 'bgrd-white-purple') : isMenClothing ? 'bgrd-navy' : 'bgrd-purple'"></span>
                    <div class="circles-rate">
                      <span class="circle" :class="this.productData.rating.rate < 0.5 ? (isMenClothing ? 'bgrd-white-navy' : 'bgrd-white-purple') : isMenClothing ? 'bgrd-navy' : 'bgrd-purple'"></span>
                      <span class="circle" :class="this.productData.rating.rate < 1.5 ? (isMenClothing ? 'bgrd-white-navy' : 'bgrd-white-purple') : isMenClothing ? 'bgrd-navy' : 'bgrd-purple'"></span>
                      <span class="circle" :class="this.productData.rating.rate < 2.5 ? (isMenClothing ? 'bgrd-white-navy' : 'bgrd-white-purple') : isMenClothing ? 'bgrd-navy' : 'bgrd-purple'"></span>
                      <span class="circle" :class="this.productData.rating.rate < 3.5 ? (isMenClothing ? 'bgrd-white-navy' : 'bgrd-white-purple') : isMenClothing ? 'bgrd-navy' : 'bgrd-purple'"></span>
                      <span class="circle" :class="this.productData.rating.rate < 4.5 ? (isMenClothing ? 'bgrd-white-navy' : 'bgrd-white-purple') : isMenClothing ? 'bgrd-navy' : 'bgrd-purple'"></span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
            <div class="mid-card">
              <p v-if="productData && productData.description">
                {{ productData.description }}
              </p>
            </div>
            <div class="bottom-card">
              <span v-if="productData && productData.price" class="price" :class="isMenClothing ? 'text-navy' : 'text-purple'">$ {{ productData.price }}</span>
              <div class="buttons">
                <button class="btn-buy" :class="isMenClothing ? 'btn-navy' : 'btn-purple'">Buy now</button>
                <button @click="displayNextProduct" class="btn-next" :class="isMenClothing ? 'btn-navy-bdr' : 'btn-purple-bdr'">Next product</button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
@import "../assets/styles/stylePage.css";
</style>
