<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number
})

const emit = defineEmits(['deleteFromDrawer'])
</script>

<template>
  <div class="border border-slate-200 p-4 rounded-xl flex gap-4">
    <img class="w-16 h-16" :src="imageUrl" :alt="title" />

    <div class="flex flex-col justify-between w-full">
      <p>{{ title }}</p>

      <div class="flex justify-between mt-2">
        <b>{{ price }} руб.</b>

        <img
          @click="() => emit('deleteFromDrawer')"
          class="opacity-40 hover:opacity-100 cursor-pointer transition"
          src="/close.svg"
          alt="close"
        />
      </div>
    </div>
  </div>
</template>
