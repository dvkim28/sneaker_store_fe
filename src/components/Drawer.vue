<script setup>
import DrawerHeadView from './DrawerHeadView.vue'
import CartListItem from './CartListItem.vue'

</script>
<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
        <DrawerHeadView />
        <CartListItem />
        <div class="flex flex-col gap-5 mt-7 mb-5">
            <div class="flex gap-2">
                <span>Total:</span>
                <div class="flex-1 border-b border-dashed border-slate-300"></div>
                <b>1200 Euro</b>
            </div>
            <div class="flex gap-2">
                <span>Tax 5%:</span>
                <div class="flex-1 border-b border-dashed border-slate-300"></div>
                <b>900 Euro</b>
            </div>
            <button 
        disabled=""
        class="transition bg-lime-500 w-full rounded-xl py-3 text-white cursor-pointer disabled:bg-slate-400 hover:bg-lime-600  active:bg-lime-700">Create order
    </button>
        </div>
    </div>
</template>