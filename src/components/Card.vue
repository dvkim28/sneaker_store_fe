<script setup>
defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onClickFavorite: Function, 
  onClickAdd: Function
})
</script>
<template>
        <div class="bg-white border-slate-100 border relative rounded-3xl p-8 transition cursor-pointer hover:-translate-y-2 hover:shadow-xl">
        <img :src="isFavorite ? '/like-2.svg' : '/like-1.svg'" alt="Like" class="absolute top-8 left-8 "@click="onClickFavorite">
        <img :src="imageUrl" alt="Sneaker">
        <p class="mt-2">{{ title }}</p>
        <div class="flex justify-between">
          <div class="flex flex-col mt-5">
            <span class="text-slate-400">Price:</span>
            <b>{{ price }} Euro</b>
          </div>
          <img :src="!isAdded? '/plus.svg' : '/checked.svg'" alt="" @click="onClickAdd">
        </div>
      </div>
</template>