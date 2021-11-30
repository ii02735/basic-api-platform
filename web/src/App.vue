<template>
    <div v-for="book in books" :v-if="books.length > 0" v-bind:key="book.id">
      <BookManagement :book="book"/>
    </div>
    <div>
         Titre : <input type="text" v-model="title"><br/>
         Auteur : <input type="text" v-model="author">
    </div>    
    <button v-on:click="createNewBook">Ajouter un livre</button>

</template>

<script>
import BookManagement from './components/BookManagement.vue'

export default {
  name: 'App',
  components: {
    BookManagement
  },
  data(){
    return {
      books: [],
      title: null,
      author: null
    }
  },
  created(){
    this.fetchBooks();
  },
  methods: {
    createNewBook(){
      fetch(process.env.VUE_APP_API+'/books', {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ title: this.title, author: this.author })
      }).then(() => {
        this.fetchBooks();
      }).catch((error) => {
        alert(error)
      })
    },
    fetchBooks(){
        fetch(process.env.VUE_APP_API+'/books').then((response) => {
        response.json().then((json) => {
          this.books = json["hydra:member"]
        })
    })
    }
  }
  
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
