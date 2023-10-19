<script setup>
    import { RouterLink, useRouter } from 'vue-router';
    import { ref, onMounted } from 'vue';
    import axios from 'axios';

    import Book from '../components/Book.vue';

    const loading = ref(true)
    const results = ref([])
    const bestDeal = ref(null)
    const route = useRouter()
    const query = ref(route.currentRoute.value.params.query)

    function getBestDeal() {
        // retrieve first item in sorted booklist
        bestDeal.value = results.value[0]

        // remove item from booklist
        results.value.shift()
    }

    function removeDuplicates() {
        const uniqueArray = []
        const seenItems = new Set()

        for (const item of results.value) {
            const itemKey = `${item.bookstore}-${item.price}-${item.title}`

            if (!seenItems.has(itemKey)) {
                seenItems.add(itemKey)
                uniqueArray.push(item)
            }
        }

        results.value = uniqueArray
    }

    async function fetch() {
        loading.value = true

        // get list of books
        results.value = []
        bestDeal.value = null
        const response = await axios.get(`https://book-browser-backend.vercel.app/api?q=${query.value}`)
        for (const bookstore in response.data) {
            for (const index in response.data[bookstore]) {
                results.value.push(response.data[bookstore][index])
            }
        }

        // sort list of books by price
        results.value.sort((currentItem, nextItem) => {
            const currentPrice = parseFloat(currentItem.price.replace('$', ''));
            const nextPrice = parseFloat(nextItem.price.replace('$', ''));
            return currentPrice - nextPrice;
        });

        // remove repeat entries
        removeDuplicates();

        getBestDeal()

        loading.value = false
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
        <div v-if="bestDeal" class="bestdeal">
            <h2 class="bestdeal-heading heading">Best deal found:</h2>
            <div class="bestdeal-book">
                <img class="bestdeal-cover" :src="bestDeal.image">
                <div class="bestdeal-content">
                    <a class="bestdeal-link" :href="`https://${bestDeal.bookstore}.com${bestDeal.url}`" target="_blank">
                        <h3 class="bestdeal-title">{{ bestDeal.title }}</h3>
                    </a>
                    <p class="bestdeal-price">${{ bestDeal.price.replace('$', '') }}</p>
                    <p class="bestdeal-format">{{ bestDeal.format }}</p>
                    <p class="bestdeal-author">{{ bestDeal.author }}</p>
                    <p class="bestdeal-bookstore">{{ bestDeal.bookstore }}</p>
                </div>
            </div>
        </div>
        <div class="search-results">
            <Book v-for="book in results" :image="book.image" :title="book.title" :url="book.url" :price="book.price"
            :format="book.format" :author="book.author" :bookstore="book.bookstore" />
        </div>
    </main>
</template>

<style scoped>
header {
    display: flex;
    align-items: center;
    padding: 0px 50px;
    gap: 20px;
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
    width: min(560px, 100%);
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
    margin: 40px 50px 50px;
    background-color: var(--text);
}

.loading-text {
    padding: 50px 0px 0px 50px;
}

.bestdeal {
    background-color: var(--contrast);
    padding: 30px;
    display: flex;
    flex-direction: column;
    align-items: center;
}

.bestdeal-heading {
    color: var(--background);
    margin: 0px 0px 20px;
}

.bestdeal-book {
    display: flex;
    gap: 30px;
    max-width: 100%;
    color: var(--background);
}

.bestdeal-link {
    color: var(--background);
}

.bestdeal-title {
    margin: 0px 0px 20px;
}

.bestdeal-title:hover {
    color: var(--emphasis);
}

.bestdeal-cover {
    max-width: 50%;
    width: 250px;
    max-height: 350px;
    aspect-ratio: 2 / 3;
    object-fit: scale-down;
    background-color: var(--contrast)
}

.bestdeal-content {
    display: flex;
    flex-direction: column;
    gap: 7px;
}

.bestdeal-content * {
    margin: 0px;
}

.bestdeal-link {
    text-decoration: none;
}

.search-results {
    display: grid;
    grid-template-columns: repeat(auto-fill, max(15%, min(100px, 28%)));
    grid-gap: max(20px, 0.5%);
    justify-content: space-between;
    padding: 50px;
}

@media (max-width: 480px) {
    header {
        padding: 0px 30px;
    }

    main {
        margin: 15px 30px 30px;
    }

    .loading-text {
        padding: 30px 0px 0px 30px;
    }

    .bestdeal-book {
        flex-direction: column;
        gap: 20px;
        width: 100%;
        color: var(--background);
    }

    .bestdeal-cover {
        max-width: 100%;
        width: 100%;
    }

    .search-results {
        grid-template-columns: repeat(auto-fill, 45.8%);
        padding: 20px;
        grid-gap: 10px;
        overflow-y:initial
    }
}
</style>