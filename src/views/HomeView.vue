<script setup>
import { useCategoryStore } from '@/stores/category';
import { useProductStore } from '@/stores/product';
import { useCartStore } from '@/stores/cart';
import { ref, } from 'vue';

const productStore = useProductStore ();
const categoryStore = useCategoryStore();
const cartStore = useCartStore();
const category = ref(null);
const searchText = ref('');
const products = ref([]);
products.value = productStore.products; 


const filterCategory = () => {
products.value = productStore.productsFilteredByCategoryName(category.value, null);
searchText.value = '';
}
const SearchName = () => {
products.value = productStore.productsFilteredByCategoryName(null, searchText.value );
category.value = '';
}

const resetAll = () => {
  searchText.value = '';
  category.value = null;
  products.value = productStore.products; 
}
</script>

<template>
  <div class="container">
    <div class="row mt-4 my-3">
      <div class="col md-5">
        <select class="form-select" aria-label="Default select example" v-model="category" @change="filterCategory">
          <option selected :value="null">Select category</option>
          <option v-for="category in categoryStore.categories" :value="category" :key="category">{{ category }}</option>
        </select>
      </div>
      <div class="col md-5">
        <input type="text" @input="searchName" class="form-control" aria-label="readonly input example" v-model="searchText">
      </div>
      <div class="col md-2">
        <button type="button" class="btn btn-outline-warning" @click="resetFilters">Reset</button>
      </div>
    </div>

    <div class="row row-cols-1 row-cols-md-3 g-4">
      <div class="col" v-for="product in products" :key="product.id">
        <div class="card">
          <img :src="product.img" class="card-img-top" alt="...">
          <div class="card-body">
            <h5 class="card-title">{{ product.name }}</h5>
            <p class="card-text">${{ product.price }}</p>
            <p class="card-text">{{ product.category }}</p>
            <p class="card-text">Date:{{ product.publish_at }}</p>
            <div class="card-footer text-end">
              <router-link :to="`/product/${product.id}`" class="btn btn-outline-info me-2">Detail</router-link>
              <button type="button" @click="cartStore.addToCart(product.id)" class="btn btn-outline-success">Cart</button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>