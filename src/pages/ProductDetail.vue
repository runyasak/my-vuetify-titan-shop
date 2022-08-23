<script setup lang="ts">
import { useFetch } from "@vueuse/core";
import { Product } from "../models/product.model";

import { useRoute } from "vue-router";

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
    <v-img :src="product?.image" width="400"></v-img>
  </v-container>
  <v-overlay v-model="isFetching" class="align-center justify-center">
    <v-progress-circular
      :size="50"
      color="secondary"
      indeterminate
    ></v-progress-circular>
  </v-overlay>
</template>
