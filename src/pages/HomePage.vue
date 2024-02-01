<script setup>
import axios from 'axios'
import { inject, reactive, watch, ref, onMounted } from 'vue';
import debounce from 'lodash.debounce';

import CardList from '../components/CardList.vue'

const { addToCart, cart } = inject('drawerActions')

const items = ref([])

const filtres = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const fetchSneakers = async () => {
  try {
    const params = {
      sortBy: filtres.sortBy
    }

    if (filtres.searchQuery) {
      params.title = `*${filtres.searchQuery}`
    }

    const { data } = await axios.get('https://437c405adcddf107.mokky.dev/sneakers', {
      params
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: cart.value.find(item => obj.id === item.id) ? true : false
    }))
  } catch (err) {
    console.log(err)
  }
}

const fecthFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://437c405adcddf107.mokky.dev/favorites')
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item.id === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item: item
      }

      item.isFavorite = true

      const { data } = await axios.post(`https://437c405adcddf107.mokky.dev/favorites`, obj)

      item.favoriteId = data.id
    } else {
      item.isFavorite = false

      await axios.delete(`https://437c405adcddf107.mokky.dev/favorites/${item.favoriteId}`)

      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

const onSortChange = (event) => {
  filtres.sortBy = event.target.value
}

const inSearchChange = debounce((event) => {
  filtres.searchQuery = event.target.value
}, 300);

onMounted(async () => {
  const localCart = localStorage.getItem('cart');
  cart.value = localCart ? JSON.parse(localCart) : [];

  await fetchSneakers()
  await fecthFavorites()
})

watch(filtres, async () => {
  await fetchSneakers()
  await fecthFavorites()
})

watch(cart, () => {
  items.value = items.value.map(item => ({
    ...item,
    isAdded: false
  }))
})
</script>

<template>
  <div class="flex justify-between items-center">
    <h2 class="text-3xl font-bold mb-8">Все кроссовоки</h2>

    <div class="flex gap-4">
      <select name="" id="" class="py-2 px-3 border rounded-md outline-none" @change="onSortChange">
        <option value="title">По названию</option>
        <option value="price">По цене(по возрастанию)</option>
        <option value="-price">По цене(по убыванию)</option>
      </select>

      <div class="relative">
        <img class="absolute left-3 top-3" src="/search.svg" />
        <input
          @input="inSearchChange"
          type="text"
          placeholder="Поиск..."
          class="border rounded-md py-1.5 pl-11 pr-4 outline-none focus:border-gray-400"
        />
      </div>
    </div>
  </div>

  <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="addToCart" />
</template>
