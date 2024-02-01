<script setup>
import axios from 'axios'
import { ref, onMounted } from 'vue'

import CardList from '@/components/CardList.vue';

const favorites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get('https://437c405adcddf107.mokky.dev/favorites')
    favorites.value = data.map(obj => obj.item)
  } catch (err) {
    console.log(err)
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>

  <CardList :items="favorites" />
</template>
