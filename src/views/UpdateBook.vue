<template>
  <div class="about">
    Form for adding a book.

    <h1 class="mb-4">Create Book</h1>
    <form action="#" method="POST" @submit.prevent="addBook">
      <div class="form-group">
        <label class="font-bold mb-2" for="title">Title</label>
        <input type="text" name="title" id="title" v-model="title">
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="author">Author</label>
        <input type="text" name="author" id="author" v-model="author">
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="image">Image</label>
        <input type="text" name="image" id="image" v-model="image">
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="description">Description</label>
        <textarea name="description" id="description" cols="30" rows="10" v-model="description"></textarea>
      </div>
      <div class="form-group">
        <label class="font-bold mb-2" for="link">Link</label>
        <input type="text" name="link" id="link" v-model="link">
      </div>
      <div class="form-group">
        <label class="font-bold mb-2"><input type="checkbox" name="featured" v-model="featured" class="mr-2">Featured</label>
      </div>
      <div class="form-group">
        <ApolloQuery :query="CategoriesQuery">
        <template slot-scope="{ result: { data, loading }, isLoading }">
            <div v-if="isLoading">Loading...</div>
            <div v-else>
                <select name="category" id="category" v-model="category">
                    <option v-for="category of data.categories" :key="category.id" :value="category.id"> {{ category.name }} </option>
                </select>
            </div>
        </template>
        </ApolloQuery>
      </div>

      <div class="form-group">
        <button type="submit">Update book</button>
      </div>

    </form>
  </div>
</template>
<script>
import CategoriesQuery from "@/graphql/queries/Categories.gql";
import BookQuery from "@/graphql/queries/Book.gql";
import UpdateBookMutation from "@/graphql/mutations/UpdateBook.gql";
export default {
    data() {
        return {
            CategoriesQuery,
            BookQuery,
            title: '',
            author: '',
            image: '',
            description: '',
            link: '',
            featured: false,
            category: 1,
            book: null
        }
    },
    apollo: {
        book: {// This updates the vue state after the query has been resolved    
            query: BookQuery,
            variables () {
                if(this.$route && this.$route.params){
                    return {
                        id: this.$route.params.id
                    }
                }
            },
            result({data, loading, networkStatus}){
                this.title = data.book.title;
                this.author = data.book.author;
                this.image = data.book.image;
                this.link = data.book.link;
                this.description = data.book.description;
                this.featured = data.book.featured;
                this.category = data.book.category.id;
                // console.log(data)
            }
        }
    },
    methods: {
        addBook() {
            this.$apollo.mutate({
                mutation: UpdateBookMutation,
                variables: {
                    id: this.$route.params.id,
                    title: this.title,
                    author: this.author,
                    image: this.image,
                    link: this.link,
                    description: this.description,
                    featured: this.featured,
                    category: this.category
                }
            })
            .then((data) => {
                // console.log(data)
                this.$router.push({name:'books', params: {id: this.$route.params.id}});
            }).catch(console.log)
        }
    }
}
</script>

<style>
    .form-group {
        margin-bottom: 32px;
    }
    input[type="text"] {
        padding: 10px 14px
    }
    button {
        padding: 16px;
        background: #027BFF;
        color: #fff;
        border-radius: 5px;
        font-size: 16px;
    }
</style>
