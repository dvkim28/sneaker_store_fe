<template>
                <div class="flex justify-between items-center">
                <h2 class="text-3xl font-bold mb-8">All Shoes</h2>
                <div class="flex gap-4">
                    <select @change="onChangeSelect" class="py-2 px-3 border border-slate-100 rounded-md outline-none">
                        <option value="title">By name</option>
                        <option value="price">By price (from cheapest)</option>
                        <option value="-price">By price (from expensive)</option>
                    </select>
                    <div class="relative">
                        <img class="absolute left-4 top-3" src="/search.svg" alt="Search">
                        <input @input="onChangeSearch"
                            class="border border-gray-200 rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-400"
                            placeholder="Search...">
                    </div>
                </div>
            </div>
            <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
</template>