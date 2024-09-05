<script setup lang="ts">
import AddToCartImage from '../assets/images/icon-add-to-cart.svg'
import DecrementIcon from './DecrementIcon.vue'
import IncrementIcon from './IncrementIcon.vue'

defineProps(['name', 'image', 'category', 'price', 'quantity', 'orderConfirmed'])
</script>

<template>
  <div class="product" :class="{ selected: orderConfirmed && quantity > 0 }">
    <div class="product_image-container">
      <img :src="image" alt="first image" class="image" />
      <div v-if="quantity !== 0" class="btn-item-added">
        <DecrementIcon @click="$emit('updateCart', name, 'reduce')" />
        <h3>{{ quantity }}</h3>
        <IncrementIcon @click="$emit('updateCart', name, 'add')" />
      </div>
      <div v-else class="btn-add" @click="$emit('updateCart', name, 'add')">
        <img :src="AddToCartImage" alt="Add To Cart Icon" />
        <h3>Add to Cart</h3>
      </div>
    </div>
    <div class="product_details">
      <h6 class="category">{{ category }}</h6>
      <p class="name">{{ name }}</p>
      <p class="price">${{ price }}</p>
    </div>
  </div>
</template>
