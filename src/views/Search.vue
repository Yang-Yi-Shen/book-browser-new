<script setup>
import { RouterLink, useRouter } from 'vue-router';
import { ref, onMounted } from 'vue';
import axios from 'axios';

import Book from '../components/Book.vue';

const loading = ref(true)
const results = ref([])
const route = useRouter()
const query = ref(route.currentRoute.value.params.query)

async function fetch() {
    loading.value = true

    console.log(query.value)
    results.value = []
    const response = await axios.get(`https://book-browser-backend.vercel.app/api?q=${query.value}`)
    for (const bookstore in response.data) {
        for (const index in response.data[bookstore]) {
            results.value.push(response.data[bookstore][index])
        }
    }
    console.log(results.value)

    loading.value = false
}

function search() {
    router.push(`/search/${query.value}`)
}

onMounted(fetch)
</script>

<template>
    <header>
        <h2 class="logo">
            <RouterLink class="logo-link heading" to="/">BB</RouterLink>
        </h2>
        <form class="search-form" @submit.prevent="search">
            <input type="text" class="searchbar" v-model="query">
            <button @click="fetch" type="submit" class="search-btn">
                <img class="search-btn-icon" src="/src/assets/search_icon.png">
            </button>
            <input type="submit" hidden>
        </form>
    </header>
    <main>
        <h2 class="loading-text" v-if="loading">Loading...</h2>
        <Book v-for="book in results" :image="book.image" :title="book.title" :url="book.url" :price="book.price"
            :format="book.format" :author="book.author" :bookstore="book.bookstore" />
    </main>
</template>

<style scoped>
header {
    display: flex;
    align-items: center;
    padding: 0px 50px;
}

.logo-link {
    text-decoration: none;
}

.search-form {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: center;
}

.searchbar {
    width: 560px;
    padding: 10px;
    border-radius: 5px;
    border: none;
    font-size: 16px;
    font-family: Arvo;
    border-radius: 5px 0 0 5px;
}

.search-btn {
    height: 40px;
    width: 40px;
    flex-shrink: 0;
    border: none;
    border-radius: 0 5px 5px 0;
    background-color: var(--contrast);
}

.search-btn:active {
    background-color: #83889d;
}

.search-btn-icon {
    height: 20px;
}

main {
    margin: 50px;
    padding: 50px;
    background-color: var(--text);
    display: flex;
    flex-wrap: wrap;
    gap: max(20px, 0.5%);
    justify-content: space-between;
}
</style>