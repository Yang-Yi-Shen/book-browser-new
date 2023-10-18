<script setup>
import { useRouter } from 'vue-router'
import { ref } from 'vue';

const query = ref()
const router = useRouter()

function search() {
    if (query.value) {
        router.push(`/search/${query.value}`)
    }
}
</script>

<template>
    <header>
        <div class="home-top">
            <h2 class="logo heading">BB</h2>
        </div>
        <div class="home-content">
            <h1 class="home-title heading">Buy books from across all platforms in one tab!</h1>
            <p class="home-subtitle">Search for books from multiple sites at once with Book Browser</p>
            <form class="search-form" @submit.prevent="search()">
                <input v-model="query" class="home-searchbar" placeholder="Find your next purchase...">
                <button type="submit" class="search-btn">
                    <img class="search-btn-icon" src="/src/assets/search_icon.png">
                </button>
            </form>
        </div>
    </header>
    <main>
        <img class="about-image" src="/src/assets/bookshelf.jpeg">
        <h1 class="about-title heading">What is Book Browser?</h1>
        <p class="about-text">Hi! I'm Yang Yi Shen. My family are avid readers, and we often have difficulty finding good
            deals when buying
            books. Our process involved opening up multiple tabs to compare prices across bookstores. Book Browser is a tool
            I created so that my family could do all of their searching in just one tab. While we still take a lot of time
            to decide which books to buy, at least now we'll have an easier time finding the cheapest deal.</p>
        <p class="about-text">If you have any suggestions for Book Browser, or just want to say hi, you can reach me at
            mrshenyangyi@gmail.com</p>
    </main>
    <footer>
        <p class="copyright">© Copyright 2023 by Yang Yi Shen</p>
    </footer>
</template>

<style scoped>
header {
    height: 100vh;
    background: none;
}

header::before {
    content: '';
    background-image: linear-gradient(transparent, var(--background) 95%), url('../assets/bibliotek.jpg');
    background-repeat: no-repeat;
    background-size: cover;
    filter: grayscale(100%);
    position: absolute;
    opacity: 0.2;
    width: 100vw;
    height: 100vh;
    z-index: -1;
}

.home-top {
    padding: 0.1px 50px;
}

.home-content {
    height: calc(100vh - 200px);
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    margin: 0px 50px;
}

.home-title {
    margin: 0px;
}

.home-subtitle {
    margin-bottom: 30px;
}

.search-form {
    display: flex;
    align-items: center;
}

.home-searchbar {
    width: min(600px, calc(100vw - 120px));
    padding: 10px;
    border-radius: 5px 0px 0px 5px;
    border: none;
    font-size: 16px;
    font-family: Arvo;
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
    margin: 0px 100px 50px;
    background-color: var(--text);
    padding: 50px;
}

main .heading {
    color: black;
}

.about-image {
    height: 180px;
    width: 100%;
    object-fit: cover;
}

footer {
    padding-bottom: 30px;
    text-align: center;
}

@media (max-width: 768px) {
    main {
        padding: 30px;
        margin: 0px 50px 50px;
    }
}
</style>