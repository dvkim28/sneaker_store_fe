<script setup>
import { onMounted, provide, ref, reactive, watch, computed } from 'vue';
import axios from 'axios';
import Header from './components/Header.vue';
import Drawer from './components/Drawer.vue';
import CardList from './components/CardList.vue';

const items = ref([]);
const cart = ref([]);
const drawerOpen = ref(false);
const isCreatingOrder = ref(false)

const closeDrawer = () => drawerOpen.value = false;
const openDrawer = () => drawerOpen.value = true;

const filters = reactive({
    sortBy: '',
    searchQuery: '',
});

const onChangeSelect = (event) => filters.sortBy = event.target.value;
const onChangeSearch = (event) => filters.searchQuery = event.target.value;

const totalPrice = computed(() => {
    return cart.value.reduce((acc, item) => acc + item.price, 0);
});

const vatPrice = computed(() => {
    return Math.round(totalPrice.value * 5) / 100;
});

const fetchFavorites = async () => {
    try {
        const { data: favorites } = await axios.get("https://cb3f8111026e789a.mokky.dev/favorites");
        items.value = items.value.map(item => {
            const favorite = favorites.find(fav => fav.parentId === item.id);
            return { ...item, isFavorite: !!favorite, favoriteId: favorite ? favorite.id : null };
        });
    } catch (error) {
        console.error(error);
    }
};

const fetchItems = async () => {
    try {
        const params = {};
        if (filters.sortBy) params.sortBy = filters.sortBy;
        if (filters.searchQuery) params.title = filters.searchQuery;

        const { data } = await axios.get("https://cb3f8111026e789a.mokky.dev/items", { params });
        items.value = data.map(obj => ({ ...obj, isFavorite: false, isAdded: false }));
    } catch (error) {
        console.error(error);
    }
};

const addToFavorite = async (item) => {
    try {
        if (!item.isFavorite) {
            const obj = { parentId: item.id };
            const { data } = await axios.post("https://cb3f8111026e789a.mokky.dev/favorites", obj);
            item.favoriteId = data.id;
            item.isFavorite = true;
        } else {
            await axios.delete(`https://cb3f8111026e789a.mokky.dev/favorites/${item.favoriteId}`);
            item.favoriteId = null;
            item.isFavorite = false;
        }
    } catch (err) {
        console.error(err);
    }
};

const addToCart = (item) => {
    cart.value.push(item);
    item.isAdded = true;
};

const removeFromCart = (item) => {
    const index = cart.value.indexOf(item);
    if (index !== -1) {
        cart.value.splice(index, 1);
        item.isAdded = false;
    }
};


const createOrder = async () => {
    try {
        isCreatingOrder.value = true
        const { data } = await axios.post(`https://cb3f8111026e789a.mokky.dev/orders`, {
            items: cart.value,
            totalPrice: totalPrice.value,
        })
        cart.value = []
        return data;
    }
    catch (err) {
        console.log(err)

    }
    finally {
        isCreatingOrder.value = false
    }
}


const onClickAddPlus = (item) => {
    if (item.isAdded) {
        removeFromCart(item);
    } else {
        addToCart(item);
    }
};

onMounted(async () => {
    const localCart = localStorage.getItem('cart')
    cart.value = localCart? JSON.parse(localCart) : [];

    await fetchItems();
    await fetchFavorites();

    items.value = items.value.map((item) => ({
        ...item,
        isAdded: cart.value.some((cartitem) => cartitem.id === item.id)
    }))
});

watch(filters, fetchItems, { deep: true });

watch(cart, () => {
    items.value = items.value.map((item) => ({
        ...item,
        isAdded: false
    }))
});

watch (cart, () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
},
{deep:true}
)

provide('cart', { closeDrawer, openDrawer, cart, addToCart, removeFromCart });
</script>

<template>
    <Drawer v-if="drawerOpen" :total-price="totalPrice" :vat-price="vatPrice" , @create-order="createOrder" :is-creating-order="isCreatingOrder" />
    <div class="bg-white w-4/5 m-auto mt-14 rounded-xl shadow-xl">
        <Header :total-price="totalPrice" @open-drawer="openDrawer" />
        <div class="p-10">
        </div>
    </div>
</template>

<style scoped></style>
