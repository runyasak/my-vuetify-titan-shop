<script setup lang="ts">
import { useFetch } from "@vueuse/core";
import { Product } from "../models/product.model";

import { useRoute } from "vue-router";
import BaseLoading from "../components/BaseLoading.vue";

const {
  params: { productId },
} = useRoute();

const { isFetching, data: product } = useFetch(
  `https://fakestoreapi.com/products/${productId}`
)
  .get()
  .json<Product>();
</script>

<template>
  <v-container>
    <v-btn variant="text" @click="$router.back()">
      <v-icon start icon="mdi-arrow-left"> </v-icon>
      Back
    </v-btn>
    <div class="wrapper">
      <div v-if="product" class="d-flex gap-12">
        <v-img :src="product.image" width="400"></v-img>
        <div class="product-description">
          <span class="text-h4">{{ product.title }}</span>
          <span>{{ product.description }}</span>
          <span class="text-h4 text-secondary font-weight-bold">
            ${{ product.price }}
          </span>

          <div class="buttons-group">
            <v-btn variant="text">buy now</v-btn>
            <v-btn color="secondary"> add to cart </v-btn>
          </div>
        </div>
      </div>
    </div>
  </v-container>
  <base-loading :loading="isFetching"></base-loading>
</template>

<style scoped>
.wrapper {
  display: flex;
  align-items: center;
  min-height: 660px;
  height: calc(100vh - 8.5em);
}

.product-description {
  display: flex;
  flex-direction: column;
  gap: 3rem;
}

.gap-12 {
  gap: 3rem;
}

.buttons-group {
  margin-top: auto;
  display: flex;
  justify-content: flex-end;
  gap: 2em;
}
</style>
