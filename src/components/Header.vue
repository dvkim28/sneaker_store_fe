<template>
<header class="flex justify-between border-b border-slate-300 px-10 py-8">
    <div class="flex items-center gap-4">
      <img src="/logo.png" alt="Logo" class="w-10">
      <div class="">
        <h2 class="text-xl font-bold uppercase">Vue Sneaker</h2>
        <p class="text-slate-400">Store of the best shoes</p>
      </div>
    </div>
     <ul class="flex items-center gap-10">
      <li class="flex items-center gap-3 cursor-pointer text-slate-500 hover:text-black">
        <img src="/cart.svg" alt="Cart">
        <span>1205 Euro</span>
      </li>
      <li class="flex items-center gap-3 cursor-pointer text-slate-500 hover:text-black">
        <img src="/heart.svg" alt="heart">
        <span>Liked</span>
      </li>
      <li class="flex items-center gap-3 cursor-pointer text-slate-500 hover:text-black">
        <img src="/profile.svg" alt="profile">
        <span>1205 Euro</span>
      </li>
    </ul>
  </header>
</template>