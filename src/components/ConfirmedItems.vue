<script setup lang="ts">
defineProps(['name', 'price', 'quantity', 'image'])
</script>

<template>
  <div class="confirmed-cart-item">
    <div class="item-detail-image">
      <img :src="image" :alt="name" />
      <div class="item-details">
        <h5 class="name">{{ name }}</h5>
        <div class="details">
          <p class="quantity">{{ quantity }}x</p>
          <p class="price">@{{ price }}</p>
        </div>
      </div>
    </div>
    <p class="total">${{ quantity * price }}</p>
  </div>
</template>
