<script setup lang="ts">
import { useFetch } from "@vueuse/core";
import { Product } from "../models/product.model";

const { isFetching, data: products } = useFetch(
  "https://fakestoreapi.com/products?limit=20"
)
  .get()
  .json<Product[]>();
</script>

<template>
  <v-container class="home-container">
    <v-row class="py-4">
      <v-col v-for="product of products" :key="product.id" col="12" md="4">
        <v-card
          class="mx-auto product-card"
          variant="elevated"
          :to="{ name: 'product-detail', params: { productId: product.id } }"
        >
          <div class="pa-4">
            <v-img
              class="product-img"
              :src="product.image"
              lazy-src="https://picsum.photos/1/1"
              height="200px"
            ></v-img>
          </div>

          <v-card-title> {{ product.title }} </v-card-title>

          <v-card-text> {{ product.price }} </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
  <v-overlay v-model="isFetching" class="align-center justify-center">
    <v-progress-circular
      :size="50"
      color="secondary"
      indeterminate
    ></v-progress-circular>
  </v-overlay>
</template>

<style scoped>
.product-img {
  transition: all 0.3s ease-in-out;
}

.product-card:hover .product-img {
  transform: scale(1.1);
}

.home-container {
  max-width: 1024px;
}
</style>
