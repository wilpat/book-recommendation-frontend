<template>
  <div class="book container">
    
    <ApolloQuery :query="require('@/graphql/queries/Book.gql')" :variables="{id: $route.params.id}">
      <template slot-scope="{ result: { data, loading }, isLoading }">
        <div v-if="isLoading">Loading...</div>
        <div v-else>
          <div v-if="data.book" class="flex flex-col lg:flex-row mt-16">
            <div>
              <img :src="`${data.book.image}`" alt="">
            </div>
            <div class="w-full lg:w-2/3 ml-0 mt-8 lg:mt-0 lg:ml-16">
              <div class="text-4xl font-bold">{{ data.book.title }}</div>
              <div class="text-2xl text-gray-800 mb-6">{{ data.book.author }}</div>
              <div class="text-lg text-gray-800 mb-6">{{ data.book.description }}</div>
              <div class="my-12">
                <a :href="data.book.link" target="_blank" class="border border-purple-600 border-solid text-purple-800 rounded px-4 py-4 mr-4 hover:bg-purple-600 hover:text-white">View Link</a>
              </div>
              <router-link :to="{name: 'update-book', params: {id: $route.params.id}}">Update</router-link>
              &middot;
              <a href="#" @click.prevent="deleteBook" >Delete</a>
            </div>
          </div>
        </div>
      </template>
    </ApolloQuery>
  </div>
</template>

<script>
import DeleteBookMutation from "@/graphql/mutations/DeleteBook.gql";
export default {
  methods: {
    deleteBook() {
      this.$apollo.mutate({
          mutation: DeleteBookMutation,
          variables: {
              id: this.$route.params.id
          }
      })
      .then((data) => {
          // console.log(data)
          this.$router.push('/');
      }).catch(console.log)
    }
  }
}
</script>
