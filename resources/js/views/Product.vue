<style scoped>
@import "./Product.scss";
</style>

<template>
  <div class="product-page" v-if="productDetail">
    <div class="product-images">
      <img :src="mainImage" alt="Product Image" class="main-image" />
      <div class="thumbnail-images">
        <img
          v-for="(image, index) in productDetail.images"
          :key="index"
          :src="image"
          @click="updateMainImage(image)"
          class="thumbnail"
        />
      </div>
    </div>
    <div class="product-details">
      <h1>{{ productDetail.name }}</h1>
      <p>
        {{ productDetail.description }}
      </p>
      <div class="price-section">
        <span class="discounted-price">{{
          productDetail.price.discounted
        }}</span>
        <span class="original-price">{{ productDetail.price.full }}</span>
        <span class="discount"
          >{{ productDetail.discount.amount }}
          {{ productDetail.discount.type == "percent" ? "%" : "Rs." }}</span
        >
      </div>
      <div class="quantity-control">
        <button @click="decreaseQuantity">-</button>
        <span>{{ quantity }}</span>
        <button @click="increaseQuantity">+</button>
      </div>
      <button class="add-to-cart">Add to cart</button>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref, onMounted } from "vue";

/*--------------------------------------------------
  Data
--------------------------------------------------*/
const mainImage = ref(null);
const quantity = ref(0);
const productDetail = ref(null);

/*--------------------------------------------------
  onMounted
--------------------------------------------------*/
onMounted(async () => {
  await getProductDetails();
});

/*--------------------------------------------------
  Methods
--------------------------------------------------*/
function updateMainImage(image) {
  mainImage.value = image;
}

function increaseQuantity() {
  quantity.value++;
}

function decreaseQuantity() {
  if (quantity.value > 0) {
    quantity.value--;
  }
}

async function getProductDetails() {
  const response = await axios.get(
    "client/products/fall-limited-edition-sneakers"
  );

  if (response.data) {
    productDetail.value = response.data.data;
    mainImage.value = productDetail.value.images[0];
  }
}
</script>
