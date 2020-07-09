<template>
  <div class="movies-index">
    <div v-for="movie in movies">
      <h1>{{ movie.title }}</h1>
      <router-link v-bind:to="`/movies/${movie.id}`">View Movie</router-link>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      movies: []
    };
  },
  created: function() {
    axios.get("/api/movies").then(response => {
      console.log("All Movies", response.data);
      this.movies = response.data;
    });
  },
  methods: {}
};
</script>