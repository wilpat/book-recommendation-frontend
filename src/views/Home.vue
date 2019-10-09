<template>
  <div class="home">
    <div class="hero bg-gray-300 mb-24">
      <div class="container flex flex-col lg:flex-row lg:justify-between py-10">
        <div class="mt-10">
          <h1 class="text-2xl font-semibold  mb-4">Book recommendation web application</h1>
          <p class="w-full lg:w-3/4 mb-6">Built with Laravel (Lighthouse Graphql), Vue.js(Vue-apollo) and Tailwind css</p>
          <div class="flex items-center">
            <a href="#" class="bg-purple-600 text-white rounded px-4 py-4 mr-4 hover:bg-purple-300">
              View Books
            </a>
            <a href="#" class="border border-purple-600 border-solid text-purple-800 rounded px-4 py-4 mr-4 hover:bg-purple-600 hover:text-white">View Screencasts</a>
          </div>
        </div>

        <div class="mt-10 lg:mt-0">
          <img src="../assets/hero.svg" alt="">
        </div>
      </div>

    </div> <!-- End hero -->

    <div class="container">
        <div class="flex flex-col lg:flex-row -mx-4">
          <!-- Left Nav -->
          <div class="w-full lg:w-1/4 px-4">
            <ApolloQuery :query="CategoriesQuery">
              <template slot-scope="{ result: { data, loading }, isLoading }">
                <div v-if="isLoading">Loading...</div>
                <ul v-else class="list-reset text-xl">
                  <li class="mb-3 hover:text-gray-800">
                    <a href="" @click.prevent="selectCategory(0)">All</a>
                  </li>

                  <li class="mb-3 hover:text-gray-800">
                    <a href="" @click.prevent="selectCategory(-1)">Featured</a>
                  </li>

                  <li class="mb-3 hover:text-gray-800" v-for="category of data.categories" :key="category.id" @click.prevent="selectCategory(category.id)">
                   <a href="#"> {{ category.name }} </a>
                  </li>

                  <li class="mb-3 hover:text-gray-800">
                     <router-link :to="{name: 'add-book'}">Add Book</router-link>
                  </li>

                </ul>
              </template>
            </ApolloQuery>
          </div>
          <!-- End left nav -->

          <!-- Books container -->
          <div class="w-full lg:w-3/4 px-4">
            <!-- All Books -->
            <ApolloQuery v-if="selectedCategory == 0" :query="query">
              <template slot-scope="{ result: { data, loading }, isLoading }">
                <div v-if="isLoading">Loading...</div>
                <div v-else>
                  <div v-if="data.books" class="flex flex-wrap">
                    <div v-for="book of data.books" :key="book.id" class="w-full lg:w-1/3 px-4 mb-12">
                      <book-listing :book="book"></book-listing>
                    </div>
                  </div>
                </div>
              </template>
            </ApolloQuery>
            
            <!-- Selected Category  -->
            <ApolloQuery v-if="selectedCategory > 0" :query="query" :variables="{id: selectedCategory}">
              <template slot-scope="{ result: { data, loading }, isLoading }">
                <div v-if="isLoading">Loading...</div>
                <div v-else>
                  <div v-if="data.category" class="flex flex-wrap">
                    <div v-for="book of data.category.books" :key="book.id" class="w-full lg:w-1/3 px-4 mb-12">
                     <book-listing :book="book"></book-listing>
                    </div>
                  </div>
                </div>
              </template>
            </ApolloQuery>

            <!-- Featured Books -->
            <ApolloQuery v-if="selectedCategory == -1" :query="query" :variables="{featured: true}">
              <template slot-scope="{ result: { data, loading }, isLoading }">
                <div v-if="isLoading">Loading...</div>
                <div v-else>
                  <div v-if="data.booksByFeatured"  class="flex flex-wrap">
                    <div v-for="book of data.booksByFeatured" :key="book.id" class="w-full lg:w-1/3 px-4 mb-12"> 
                      <book-listing :book="book"></book-listing>
                    </div>
                  </div>
                </div>
              </template>
            </ApolloQuery>
              
          </div>
          
        </div>
    </div>

  </div>
</template>

<script>
// @ is an alias to /src
import BooksQuery from "@/graphql/queries/Books.gql";
import CategoriesQuery from "@/graphql/queries/Categories.gql";
import CategoryQuery from "@/graphql/queries/Category.gql";
import BooksByFeaturedQuery from "@/graphql/queries/BooksByFeatured.gql";
import bookListing from "@/components/bookListing";
export default {
  name: 'home',
  components: {
    bookListing
  },
  data () {
    return {
      selectedCategory: 0, // 0: All, -1 : Featured, >0: SelectedCategory
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
