<script setup>
import { computed, ref, inject } from 'vue'
import axios from 'axios'

import DrawerHead from './DrawerHead.vue'
import DrawerList from './DrawerList.vue'
import InfoBlock from './InfoBlock.vue'

const props = defineProps({
  totalPrice: Number
})

const isCreatingOrder = ref(false)
const orderId = ref(null)
const { cart } = inject('drawerActions')

const vatPrice = computed(() => {
  return Math.round(props.totalPrice* 0.05)
})

const buttonDisabled = computed(() =>
  isCreatingOrder.value ? true : props.totalPrice ? false : true
)

const createOrder = async () => {
  try {
    isCreatingOrder.value = true
    const { data } = await axios.post('https://437c405adcddf107.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })

    cart.value = []

    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false
  }
}
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8 flex flex-col">
    <DrawerHead />

    <InfoBlock
      v-if="!totalPrice && orderId"
      title="Заказ оформлен!"
      :description="`Ваш заказ ${ orderId } скоро будет передан курьерской доставке`"
      image-url="order-success-icon.png"
    />

    <InfoBlock
      v-else-if="!totalPrice && !orderId"
      title="Корзина пуста"
      description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ."
      image-url="package-icon.png"
    />
    
    <div v-else class="flex flex-col h-full">
      <DrawerList />

      <div class="flex flex-col gap-4 my-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} ₽</b>
        </div>

        <div class="flex gap-2">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} ₽</b>
        </div>
        <button
          :disabled="buttonDisabled"
          class="bg-lime-500 rounded-xl w-full py-3 text-white disabled:bg-slate-400 cursor-pointer hover:bg-lime-600 transition active:bg-lime-700"
          @click="createOrder"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
