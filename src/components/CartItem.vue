<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number, 
})



</script>
<template>
  <div class="flex items-center border border-slate-100 p-4 rounded-xl gap-4">
    <img :src="imageUrl" class="w-16 h-16" :alt="title">

    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>
      <div class="flex justify-between items-center mt-2">
        <b class="flex flex-1">{{ price }} Euro</b>
        <img @click="emit('onClickRemove')" src="/close.svg"
          class="opacity-60 hover:opacity-100 cursor-pointer transition">
      </div>
    </div>
  </div>
</template>