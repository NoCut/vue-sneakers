<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
})

const emits = defineEmits(['onClickFavorite', 'onClickAdd'])
</script>

<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition flex flex-col"
  >
    <img
      @click="() => emits('onClickFavorite')"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="Like 1"
      class="absolute top-8 left-8"
      v-if="$route.path !== '/favorites'"
    />
    <img :src="imageUrl" alt="Sneaker" class="sneaker-img" />

    <p class="mt-2 flex-1">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена: </span>
        <b>{{ price }} руб.</b>
      </div>

      <img
        @click="() => emits('onClickAdd')"
        :src="!isAdded ? '/plus.svg' : '/checked.svg'"
        alt="Plus"
        v-if="$route.path !== '/favorites'"
      />
    </div>
  </div>
</template>

<style scope>
.sneaker-img {
  max-width: 250px;
  max-height: 160px;
  margin: auto;
}
</style>
