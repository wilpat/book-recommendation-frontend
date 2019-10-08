<template>
  <div class="about">
    
    <ApolloQuery :query="require('@/graphql/queries/Book.gql')" :variables="{id: $route.params.id}">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-if="data.book">
              <div>{{ data.book.title }}</div>
              <div>{{ data.book.author }}</div>
              <img :src="`${data.book.image}`" alt="">
              <div>
                <router-link class="link-margin" :to="{name: 'update-book', params: {id: $route.params.id}}">Update</router-link>
                <router-link class="link-margin" :to="{}">Delete</router-link>
              </div>
          </div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>
