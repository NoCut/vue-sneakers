<script setup>
import CardElement from './CardElement.vue'

defineProps({
  items: Array,
})

const emit = defineEmits(["addToFavorite", "addToCart"]);
</script>

<template>
  <div class="grid grid-cols-4 gap-5 mt-10" v-auto-animate>
    <CardElement
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :imageUrl="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :isAdded="item.isAdded"
      @on-click-add="() => emit('addToCart', item)"
      :isFavorite="item.isFavorite"
      @on-click-favorite="() => emit('addToFavorite', item)"
    />
  </div>
</template>
