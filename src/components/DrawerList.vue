<script setup>
import { inject } from 'vue'
import DrawerItem from './DrawerItem.vue'

const { addToCart, cart } = inject("drawerActions")
</script>

<template>
  <div class="flex flex-col gap-4 flex-1" v-auto-animate>
    <DrawerItem 
      v-for="item in cart"
      :key="item.id"
      :id="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.price"
      @delete-from-drawer="() => addToCart(item)"
    />
  </div>
</template>
