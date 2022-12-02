<script setup>
import { ref, watch } from 'vue'

const searchTerm = ref('')

const debounce = ref(null)
const loading = ref(false)

const products = ref([])

const findProducts = async term => {
  clearTimeout(debounce.value)

  products.value = []

  if (searchTerm.value.length < 1) {
    loading.value = false
    return
  }

  loading.value = true

  debounce.value = setTimeout(async () => {
    products.value = []

    try {
      const response = await fetch(`https://dummyjson.com/products/search?q=${term}&limit=4`)
      const data = await response.json()

      if (searchTerm.value.length < 1) {
        loading.value = false
        return
      }
      products.value = data.products

      loading.value = false
    } catch (error) {
      alert("Something went wrong")
      console.error(error)
    }
  }, 300)
}

const currency = value => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: 'USD'
  }).format(value)
}

watch(searchTerm, newTerm => findProducts(newTerm))
</script>

<template>
  <div class="w-full h-full flex flex-col gap-5 justify-center items-center">
    <h1 class="text-4xl font-bold">Gift Search Bar</h1>
    <input type="search" class="p-2 border-2 border-gray-dark" v-model="searchTerm" placeholder="Start typing..." />
    <ul class="list-none flex flex-col gap-5">
      <li v-if="loading">
        <p>Loading...</p>
      </li>
      <li v-for="(product, index) in products" class="flex items-center gap-4">
        <img width="75" height="75" :src="product.images[0]" :alt="product.title">

        <div>
          <h2 class="text-xl font-bold">{{ product.title }}</h2>
          <h3 class="text-gray-dark">
            {{ currency(product.price) }}
          </h3>
        </div>
      </li>
    </ul>
  </div>
</template>
