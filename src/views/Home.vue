<template>
  <div class="home">
    <ApolloQuery :query="require('@/graphql/queries/Categories.gql')">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <a href="#" v-for="category of data.categories" :key="category.id" class="link-margin" @click="selectCategory(category.id)">
            {{ category.id }}. {{ category.name }}
          </a>
        </div>
      </template>
    </ApolloQuery>

    <ApolloQuery :query="require('@/graphql/queries/Books.gql')" v-if="selectedCategory == 0">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-for="book of data.books" :key="book.id">
            {{ book.id }}. {{ book.title }}
          </div>
        </div>
      </template>
    </ApolloQuery>

    
    <ApolloQuery :query="require('@/graphql/queries/Category.gql')" :variables="{id: selectedCategory}" v-else>
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-for="book of data.category.books" :key="book.id">
            {{ book.id }}. {{ book.title }}
          </div>
        </div>
      </template>
    </ApolloQuery>


  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'home',
  components: {

  },
  data () {
    return {
      selectedCategory: 0
    }
  },

  methods: {
    selectCategory(id) {
      this.selectedCategory = id;
    }
  }
}
</script>

<style>
  .link-margin{
    margin-right: 24px;
  }
</style>
