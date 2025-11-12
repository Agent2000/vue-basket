<script setup>
import { computed, reactive } from 'vue'

const basket = reactive([
  {
    id: 1,
    name: 'Blue Flower Print Crop Top',
    color: 'Yellow',
    size: 'M',
    price: 29.0,
    quantity: 1,
    imageUrl: '../public/crop-top.png',
  },
  {
    id: 2,
    name: 'Levender Hoodie',
    color: 'Levender',
    size: 'XXL',
    price: 119.0,
    quantity: 1,
    imageUrl: '../public/hoodie.png',
  },
  {
    id: 3,
    name: 'Black Sweatshirt',
    color: 'Black',
    size: 'XXL',
    price: 123.0,
    quantity: 1,
    imageUrl: '../public/sweatshirt.png',
  },
])

const increaseItemQuantity = (item) => {
  item.quantity++
}

const decreaseItemQuantity = (item) => {
  item.quantity > 0 ? item.quantity-- : 0
}

const totalItemPrice = (item) => {
  return Number(item.price) * item.quantity
}

const removeItem = (index) => {
  basket.splice(index, 1)
}

const isEmptyBasket = computed(() => {
  return basket.length === 0
})

const totalPrice = computed(() => {
  return basket.reduce((sum, { price, quantity }) => sum + price * quantity, 0).toFixed(2)
})

const totalTax = computed(() => {
  const _totalPrice = basket
    .reduce((sum, { price, quantity }) => sum + price * quantity, 0)
    .toFixed(2)
  console.log(_totalPrice)
  return (_totalPrice * 0.1).toFixed(2)
})
</script>

<template>
  <div class="container basket">
    <table class="basket-table">
      <thead class="basket-table__header">
        <tr>
          <th>Product Details</th>
          <th>Price</th>
          <th>Quantity</th>
          <th>Subtotal</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody class="basket-table__body">
        <tr v-for="(product, index) in basket" :key="product.id">
          <td>
            <div class="basket-item">
              <div class="basket-item__image">
                <img :src="`${product.imageUrl}`" alt="" />
              </div>
              <div class="basket-item__info">
                <h2 class="basket-item__info-h2">{{ product.name }}</h2>
                <p class="basket-item__info-p">{{ product.color }}</p>
                <p class="basket-item__info-p">{{ product.size }}</p>
              </div>
            </div>
          </td>
          <td>
            <p class="basket-item__price">${{ product.price }}</p>
          </td>
          <td>
            <div class="basket-item__quantity">
              <button @click="decreaseItemQuantity(product)" class="quantity-button">–</button>
              <input type="number" :value="`${product.quantity}`" min="1" />
              <button @click="increaseItemQuantity(product)" class="quantity-button">+</button>
            </div>
          </td>
          <td>
            <p class="basket-item__price">${{ totalItemPrice(product) }}</p>
          </td>
          <td>
            <button @click="removeItem(index)" class="btn btn-delete" aria-label="Удалить">
              <svg
                class="w-6 h-6 text-gray-800 dark:text-white"
                aria-hidden="true"
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                fill="none"
                viewBox="0 0 24 24"
              >
                <path
                  stroke="currentColor"
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  stroke-width="2"
                  d="M5 7h14m-9 3v8m4-8v8M10 3h4a1 1 0 0 1 1 1v3H9V4a1 1 0 0 1 1-1ZM6 7h12v13a1 1 0 0 1-1 1H7a1 1 0 0 1-1-1V7Z"
                />
              </svg>
            </button>
          </td>
        </tr>
        <tr>
          <td v-if="isEmptyBasket" colspan="5">
            <p class="basket-table__empty">No items</p>
          </td>
        </tr>

        <tr v-if="!isEmptyBasket">
          <td colspan="5">
            <div class="basket-table__summary">
              <p class="basket-table__total">
                Total <b>${{ totalPrice }}</b>
              </p>
              <p>Tax ${{ totalTax }}</p>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style src="./App.css"></style>
