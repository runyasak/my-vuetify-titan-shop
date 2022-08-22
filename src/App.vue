<script setup lang="ts">
import { useFetch } from "@vueuse/core";
import { Product } from "./models/product.model";

const { isFetching, data: products } = useFetch(
  "https://fakestoreapi.com/products?limit=20"
)
  .get()
  .json<Product[]>();
</script>

<template>
  <v-app>
    <v-main>
      <v-app-bar color="#15202B">
        <v-app-bar-title
          class="text-center font-weight-bold text-h4 text-white"
        >
          MY SHOP
        </v-app-bar-title>
      </v-app-bar>
      <v-container>
        <v-row class="py-4">
          <v-col v-for="product of products" :key="product.id" col="12" md="4">
            <v-card class="mx-auto" :variant="'elevated'">
              <v-img :src="product.image" height="200px"></v-img>

              <v-card-title> {{ product.title }} </v-card-title>

              <v-card-text> {{ product.price }} </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>

    <v-overlay v-model="isFetching" class="align-center justify-center">
      <v-progress-circular
        :size="50"
        color="secondary"
        indeterminate
      ></v-progress-circular>
    </v-overlay>
  </v-app>
</template>
