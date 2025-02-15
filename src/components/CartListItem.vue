<script setup>
import CartItem from './CartItem.vue'

defineProps({
  item : Array
})

</script>

<template>
  <div class="grid flex-co flex-1 gap-4">
    <CartItem /> 
    <CartItem /> 
    <CartItem /> 
    <CartItem /> 
    <CartItem /> 
    <CartItem /> 

    </div>
</template>