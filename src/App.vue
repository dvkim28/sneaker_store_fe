<script setup>
import { onMounted, ref, reactive, watch, provide } from 'vue';
import axios from 'axios';
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'

const items = ref([]);

const filters = reactive({
    sortBy: '',
    searchQuary: '',
});

const OnChangeSelect = (event) => {
    filters.sortBy = event.target.value
}

const OnChangeSearch = (event) => {
    filters.searchQuary = event.target.value
}

const fetchFavorites = async() => {
    try {
        const { data:favorites  } = await axios.get("https://cb3f8111026e789a.mokky.dev/favorites");
        items.value = items.value.map(item =>{
            const favorite = favorites.find(favorite => favorite.parentId === item.id);
            if (!favorite) {
                return item
            }
            return{
                ...item,
                isFavorite: true,
                favoriteId: favorite.id,
            }
        });
    } catch (error) {
        console.log(error);
    }
}

const fetchItems = async () => {
    const params = {
        sortBy: filters.sortBy,
    }

    if (filters.searchQuary) {
        params.title = `*${filters.searchQuary}*`; 
    }

    try {
        const { data } = await axios.get("https://cb3f8111026e789a.mokky.dev/items", { params });
        items.value = data.map(obj =>({
            ...obj,
            isFavorite: false,
            isAdded: false,
        }));
    } catch (error) {
        console.log(error);
    }
}

const addToFavorite = async (item) => {
    item.isFavorite = !item.isFavorite
}

onMounted(async () => {
    await fetchItems();
    await fetchFavorites();
})
watch(filters, fetchItems)

provide('addToFavorite',  addToFavorite);

</script>


<template>
    <!-- <Drawer /> -->
    <div class="bg-white w-4/5 m-auto mt-14 rounded-xl shadow-xl">
        <Header />

        <div class="p-10">
            <div class="flex justify-between items-center">
                <h2 class="text-3xl font-bold mb-8">All Shoes</h2>

                <div class="flex gap-4">
                    <select @change=OnChangeSelect class="py-2 px-3 border border-slate-100 rounded-md outline-none"
                        id="">
                        <option value="title">By name</option>
                        <option value="price">By price (from cheapest)</option>
                        <option value="-price">By price (from expensive)</option>


                    </select>
                    <div class="relative">
                        <img class="absolute  left-4 top-3" src="/search.svg" alt="">
                        <input
                        @input=OnChangeSearch
                            class="border border-gray-200 rounded-md py-2 pl-10 pr-4 outline-none focus:border-gray-400"
                            placeholder="Search...">
                    </div>
                </div>


            </div>
            <CardList :items="items" />

        </div>
    </div>
</template>

<style scoped></style>
