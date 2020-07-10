<template>
  <div class="movies-index">

    Search by title: <input v-model="titleFilter" list="titles">
    <div>
    <button>Sort Alphabetically</button>
    </div>
    <div v-for="movie in orderBy(filterBy(movies, titleFilter, 'title'), 'title')">
      <h1>{{ movie.title }}</h1>
      <router-link v-bind:to="`/movies/${movie.id}`">View Movie</router-link>
    </div>
    <datalist id="titles">
      <option v-for="movie in movies">{{ movie.title }}</option>
    </datalist>

  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";
export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      movies: [],
      titleFilter: ""
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