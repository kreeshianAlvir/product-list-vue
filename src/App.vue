<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import ProductList from './components/ProductList.vue'
import CartList from './components/CartList.vue'
import ConfirmedItems from './components/ConfirmedItems.vue'

import Data from './assets/json/data.json'
import IconEmptyCart from './components/icons/IconEmptyCart.vue'
import IconCarbonNeutral from './components/icons/IconCarbonNeutral.vue'
import IconOrderConfirmed from './components/icons/IconOrderConfirmed.vue'

interface FoodData {
  name: string
  image: {
    thumbnail: string
    mobile: string
    tablet: string
    desktop: string
  }
  category: string
  price: number
  quantity?: number
}

const FoodList = ref<FoodData[]>([])
const totalSelectedItems = ref<number>(0)
const totalPrice = ref<number>(0)
const orderConfirmed = ref<boolean>(false)

onMounted(() => {
  Data.forEach((item: FoodData) => {
    item.quantity = 0
  })
  FoodList.value = Data
})

function updateCart(itemName: string, action: string) {
  let list = FoodList.value

  list.forEach((item: FoodData) => {
    if (itemName === item.name) {
      const quantity = item.quantity!

      switch (action) {
        case 'add':
          item.quantity = quantity + 1
          break
        case 'reduce':
          item.quantity = quantity !== 0 ? quantity - 1 : 0
          break
        case 'remove':
          item.quantity = 0
          break
        default:
          break
      }
    }
  })

  FoodList.value = [...list]
}

function showConfirmedOrders() {
  orderConfirmed.value = true
}

function startNewOrder() {
  let list = FoodList.value

  list.forEach((item: FoodData) => {
    item.quantity = 0
  })

  FoodList.value = [...list]
  orderConfirmed.value = false
}

watch(FoodList, () => {
  // get the total quantity of the selected items
  const quantityArray =
    FoodList.value.length !== 0
      ? FoodList.value.map((n) => {
          return n.quantity
        })
      : [0]

  totalSelectedItems.value =
    quantityArray.reduce((quantity, total) => {
      return total! + quantity!
    }) || 0

  // get total order price
  const allItemsSelected = FoodList.value.filter((n) => n.quantity !== 0)
  const itemPrices =
    allItemsSelected.length !== 0
      ? allItemsSelected.map((n) => {
          return n.price * n.quantity!
        })
      : [0]

  totalPrice.value = itemPrices.reduce((price, total) => {
    return total + price
  })
})
</script>

<template>
  <section class="list-section">
    <h1 class="section-header_list">Desserts</h1>
    <!-- the list -->
    <section class="product-list">
      <ProductList
        v-for="food in FoodList"
        :key="food.name"
        :name="food.name"
        :image="food.image.desktop"
        :category="food.category"
        :price="food.price"
        :quantity="food.quantity"
        :orderConfirmed="orderConfirmed"
        @updateCart="updateCart"
      />
    </section>
  </section>

  <section class="cart-section">
    <h1 class="section-header_cart">Your Cart ({{ totalSelectedItems }})</h1>
    <section>
      <CartList
        v-if="totalSelectedItems !== 0"
        v-for="food in FoodList.filter((n) => n.quantity !== 0)"
        :name="food.name"
        :price="food.price"
        :quantity="food.quantity"
        @updateCart="updateCart"
      />
      <div v-if="totalSelectedItems !== 0">
        <div class="total-price">
          <p>Order Total</p>
          <p class="total-order-price">${{ totalPrice }}</p>
        </div>
        <div class="carbon-neutral">
          <IconCarbonNeutral />
          <p class="carbon-hint">This is a <strong>carbon-neutral</strong> delivery</p>
        </div>
        <button class="btn-order-complete" @click="showConfirmedOrders">Confirm Order</button>
      </div>
      <div v-else class="empty-cart-section">
        <IconEmptyCart />
        <p>Your added items will appear here</p>
      </div>
    </section>
  </section>

  <!-- backdrop -->
  <!-- dialog -->
  <div v-if="orderConfirmed" class="dialog-backdrop">
    <div class="dialog">
      <IconOrderConfirmed />
      <h1 class="title">Order Confirmed</h1>
      <p class="subtitle">We hope you enjoy your food!</p>
      <div class="order-list-total">
        <div class="confirmed-order-list">
          <ConfirmedItems
            v-for="food in FoodList.filter((n) => n.quantity !== 0)"
            :image="food.image.thumbnail"
            :name="food.name"
            :price="food.price"
            :quantity="food.quantity"
          />
        </div>
        <div class="confirmed-price-total">
          <p class="label-price">Order Total</p>
          <p>${{ totalPrice }}</p>
        </div>
      </div>
      <button class="btn-order-complete" @click="startNewOrder">Start New Order</button>
    </div>
  </div>
</template>
