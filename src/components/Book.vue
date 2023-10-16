<script setup>
defineProps({
    image: String,
    title: String,
    url: String,
    price: String,
    format: String,
    author: String,
    bookstore: String
})
</script>

<template>
    <div class="book">
        <img class="cover" :src="image">
        <a class="link" :href="`https://${bookstore}.com${url}`">
            <p class="title">{{ title }}</p>
        </a>
        <p class="price">${{ price.replace('$', '') }}</p>
        <p class="format">{{ format }}</p>
        <p class="author">{{ author }}</p>
        <p class="bookstore">{{ bookstore }}</p>
    </div>
</template>

<style scoped>
.book {
    flex: 0 1 15%;
    display: inline-block;
}

.cover {
    width: 100%;
}
</style>