<template>
    <div>
        <router-link :to="{name:'books', params:{'id': book.id}}">
        <img :src="`${book.image}`" alt="" class="h-64 mb-2">
        </router-link>

        <router-link :to="{name:'books', params:{'id': book.id}}" class="text-lg font-bold text-black hover:text-gray-800 mb-1"> 
        {{ book.title }}
        </router-link>
        <div class="text-gray-800">{{ book.author }}</div>
    </div>
</template>

<script>
export default {
    props: ['book']
}
</script>