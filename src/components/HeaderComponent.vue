<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light bg_header">
      <div class="container-fluid d-flex justify-content-around align-items-center">
          <img src="https://fontmeme.com/temporary/54a71b402142c0c7c2639b1407f95fe7.png" alt="">
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarSupportedContent"
          aria-controls="navbarSupportedContent"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
        <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse justify-content-end mt-3" id="navbarSupportedContent">     
          <form class="d-flex" @submit.prevent>
            <input
              class="form-control me-2"
              type="search"
              placeholder="Search"
              aria-label="Search"
              v-model="inputSearch"
            />
            <button class="btn btn-outline-success"  @click="search" type="submit">Search</button>
          </form>
        </div>
      </div>
    </nav>
    <div class="container-fluid">
        <div class="row">
            <div class="col-3 card d-flex" v-for="(movie, index) in movies" :key="index">
                <img :src="'https://image.tmdb.org/t/p/w342' + movie.poster_path" alt="">
                <div>{{movie.title}}</div>
                <div>{{movie.original_title}}</div>
                <div>{{movie.original_language}}</div> <lang-flag :iso="movie.original_language" :squared="true"/>
                <div>{{movie.vote_average}}</div>
                
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "SiteHeader",
  data(){
      return {
          movies: [],
          error: null,
          query: "",
          inputSearch: ""
      }
  },
   methods:{
     search() {
        console.log('Searching ...');
       this.query = this.inputSearch
       axios.get(`https://api.themoviedb.org/3/search/movie?api_key=6492e216147f755d586c53abf227d9a5&language=it-IT&page=1&include_adult=false&query=${this.query}`)
      .then((results) =>{
          this.movies = results.data.results;
      })
      .catch((error) => {
          console.log(error);
          this.error = `Scusa c'è un errore ${this.error}`
      })
    }
  },
};
</script>

<style lang="scss" scoped>
.bg_header{
    background-color: #222 !important;
}
</style>