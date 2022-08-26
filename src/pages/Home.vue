<script setup lang="ts">
import { useFetch } from "@vueuse/core";
import { Product } from "../models/product.model";
import BaseLoading from "../components/BaseLoading.vue";
import { computed, ref } from "@vue/reactivity";

const { isFetching, data: products } = useFetch(
  "https://fakestoreapi.com/products?limit=20"
)
  .get()
  .json<Product[]>();

const query = ref("");

const searchProducts = computed(() =>
  products.value?.filter((product) =>
    product.title.toLowerCase().includes(query.value.toLowerCase())
  )
);
</script>

<template>
  <v-container class="home-container">
    <v-text-field v-model="query"></v-text-field>
    <v-row class="py-4">
      <v-col
        v-for="product of searchProducts"
        :key="product.id"
        col="12"
        md="4"
      >
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

          <v-card-text class="text-body-1 text-secondary font-weight-bold">
            ${{ product.price }}
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
  <base-loading :loading="isFetching"></base-loading>
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
