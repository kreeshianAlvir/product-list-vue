<script setup lang="ts">
import ItemRemoveIcon from '../components/icons/IconItemRemove.vue'
defineProps(['name', 'price', 'quantity'])
</script>

<template>
  <div class="cart-item">
    <div class="item-details">
      <h5 class="name">{{ name }}</h5>
      <div class="details">
        <p class="quantity">{{ quantity }}x</p>
        <p class="price">@{{ price }}</p>
        <p class="total">${{ quantity * price }}</p>
      </div>
    </div>
    <ItemRemoveIcon class="btn-remove" @click="$emit('updateCart', name, 'remove')" />
  </div>
</template>
