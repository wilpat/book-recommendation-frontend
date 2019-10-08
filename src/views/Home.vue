<template>
  <div class="home">
    <router-link :to="{name: 'add-book'}">Add Book</router-link>
    <ApolloQuery :query="CategoriesQuery">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <a href="#" @click.prevent="selectCategory(0)" class="link-margin">All</a>
          <a href="#" @click.prevent="selectCategory(-1)" class="link-margin">Featured</a>
          <a href="#" v-for="category of data.categories" :key="category.id" class="link-margin" @click.prevent="selectCategory(category.id)">
            {{ category.id }}. {{ category.name }}
          </a>
        </div>
      </template>
    </ApolloQuery>

    <ApolloQuery v-if="selectedCategory == 0" :query="query">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-if="data.books">
            <div v-for="book of data.books" :key="book.id">
              <router-link :to="{name:'books', params:{'id': book.id}}"> 
                {{ book.id }}. {{ book.title }}
              </router-link>
              <div>{{ book.author }}</div>
              <img :src="`${book.image}`" alt="">
            </div>
          </div>
        </div>
      </template>
    </ApolloQuery>

    <ApolloQuery v-if="selectedCategory == -1" :query="query" :variables="{featured: true}">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-if="data.booksByFeatured">
            <div v-for="book of data.booksByFeatured" :key="book.id">
              <router-link :to="{name:'books', params:{'id': book.id}}"> 
                {{ book.id }}. {{ book.title }}
              </router-link>
              <div>{{ book.author }}</div>
              <img :src="`${book.image}`" alt="">
            </div>
          </div>
        </div>
      </template>
    </ApolloQuery>

    <ApolloQuery v-if="selectedCategory > 0" :query="query" :variables="{id: selectedCategory}">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-if="data.category">
            <div v-for="book of data.category.books" :key="book.id">
              <router-link :to="{name:'books', params:{'id': book.id}}"> 
                {{ book.id }}. {{ book.title }}
              </router-link>
              <div>{{ book.author }}</div>
              <img :src="`${book.image}`" alt="">
            </div>
          </div>
        </div>
      </template>
    </ApolloQuery>


  </div>
</template>

<script>
// @ is an alias to /src
import BooksQuery from "@/graphql/queries/Books.gql";
import CategoriesQuery from "@/graphql/queries/Categories.gql";
import CategoryQuery from "@/graphql/queries/Category.gql";
import BooksByFeaturedQuery from "@/graphql/queries/BooksByFeatured.gql";
export default {
  name: 'home',
  components: {

  },
  data () {
    return {
      selectedCategory: 0,
      query:BooksQuery,
      BooksQuery,
      CategoriesQuery,
      CategoryQuery,
      BooksByFeaturedQuery
    }
  },

  methods: {
    selectCategory(category) {
      if(category === 0){
        this.query = BooksQuery;
      } else if(category === -1) {
        this.query = BooksByFeaturedQuery;
      } else{
        this.query = CategoryQuery;
      }
      this.selectedCategory = parseInt(category);
    },
    
  }
}
</script>

<style>
  .link-margin{
    margin-right: 24px;
  }
</style>
