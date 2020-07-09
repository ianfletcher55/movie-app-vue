<template>
  <div class="movies-show">
    <h2>{{ movie.title }}</h2>
    <h4>{{ movie.director }}</h4>
    <h6>{{ movie.year }}</h6>
    <p>{{ movie.plot }}</p>
    <p>{{ movie.enligh }}</p>
    <p></p>
    <router-link :to="`/movies/${movie.id}/edit`">Edit movie</router-link> <br>
    <button v-on:click="destroyMovie()">Delete Movie</button>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      movie: {}
    };
  },
  created: function() {
    axios.get(`/api/movies/${this.$route.params.id}`).then(response => {
      console.log(response.data);
      this.movie = response.data;
    });
  },
  methods: {
    destroyMovie: function() {
      if (confirm("Are you sure you want to delete this movie?")) {
        axios.delete(`/api/movies/${this.movie.id}`).then(response => {
          console.log("Succesfully deleted movie", response.data);
          this.$router.push("/movies");
        });
      }
    }
  }
};
</script>