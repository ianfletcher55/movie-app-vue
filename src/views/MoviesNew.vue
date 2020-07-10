<template>
  <div class="movies-new">
    <form v-on:submit.prevent="createMovie()">
      <h1>New Movie</h1>
      <ul>
        <li class="text-danger" v-for="error in errors">{{ error }}</li>
      </ul>

      <div class="form-group">
        <label>Title:</label>
        <input type="text" class="form-control" v-model="title">
      </div>

      <div class="form-group">
        <label>Director:</label>
        <input type="text" class="form-control" v-model="director">
      </div>

      <div class="form-group">
        <label>Year:</label>
        <input type="text" class="form-control" v-model="year">
      </div>

      <div class="form-group">
        <label>Plot:</label>
        <input type="text" class="form-control" v-model="plot"> <br>
        <small>{{ 250 - plot.length }} characters remaining</small>
      </div>

      <div class="form-group">
        <label>English:</label>
        <input type="text" class="form-control" v-model="english">
      </div>

      <input type="submit" class="btn btn-primary" value="Create">

    </form>

  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      title: "",
      director: "",
      year: "",
      plot: "",
      english: "",
      errors: []
    };
  },
  created: {},
  methods: {
    createMovie() {
      var params = {
        title: this.title,
        director: this.director,
        year: this.year,
        plot: this.plot,
        english: this.english
      };

      axios
        .post("/api/movies", params)
        .then(response => {
          this.$router.push("/movies");
        })
        .catch(error => {
          this.errors = error.response.data.errors;
        });
    }
  }
};
</script>