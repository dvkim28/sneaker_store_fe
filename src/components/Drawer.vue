<script setup>
import DrawerHeadView from './DrawerHeadView.vue'
import CartListItem from './CartListItem.vue'
import {computed } from 'vue';
import infoBlock from './infoBlock.vue'


const props = defineProps({
    vatPrice: Number,
    totalPrice: Number,
    isCreatingOrder: Boolean,
})

const emit = defineEmits(['createOrder'])

const buttonDissabled = computed(() => props.isCreatingOrder.value? true : props.totalPrice ? false : true)

</script>
<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8" v-auto-animate>
        <DrawerHeadView />
        <infoBlock v-if="!totalPrice" title="the box is empty" description="Add few more items" image-url="/package-icon.png" />
        <CartListItem v-if="totalPrice" />
        <div v-if="totalPrice" class="flex flex-col gap-5 mt-7 mb-5">
            <div class="flex gap-2">
                <span>Total:</span>
                <div class="flex-1 border-b border-dashed border-slate-300"></div>
                <b>{{totalPrice}} Euro</b>
            </div>
            <div class="flex gap-2">
                <span>Tax 5%:</span>
                <div class="flex-1 border-b border-dashed border-slate-300"></div>
                <b>{{vatPrice}} Euro</b>
            </div>
            <button 
        :disabled="buttonDissabled"
        @click="() => emit('createOrder')"
        class="transition bg-lime-500 w-full rounded-xl py-3 text-white cursor-pointer disabled:bg-slate-400 hover:bg-lime-600  active:bg-lime-700">
        Create order
    </button>
        </div>
    </div>
</template>