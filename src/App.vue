<script setup>
import { ref, provide, watch, computed } from 'vue'

import HeaderSection from './components/HeaderSection.vue'
import DrawerSection from './components/DrawerSection.vue'

const cart = ref([])
const drawerOpen = ref(false)

const totalPrice = computed(() => {
  return cart.value.reduce((acc, item) => acc + item.price, 0)
})

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const addToCart = (item) => {
  if (item.isAdded) {
    cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false
  } else {
    cart.value.push(item)
    item.isAdded = true
  }
}

watch(
  cart,
  async () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  {
    deep: true
  }
)

provide('drawerActions', { 
  closeDrawer,
  openDrawer,
  addToCart,
  cart
})

</script>

<template>
  <DrawerSection
    v-if="drawerOpen"
    :total-price="totalPrice"
  />

  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl my-14">
    <HeaderSection @open-drawer="openDrawer" :total-price="totalPrice"/>

    <main class="p-10">
      <router-view></router-view>
    </main>
  </div>
</template>
