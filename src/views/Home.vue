<template>
  <div class="home">
    <h1>{{ message }}</h1>

    <ul>
      <li v-for="error in errors">{{ error }}</li>
    </ul>

    <div>
      Title: <input type="text" v-model="newMovieTitle"> <br>
      Year: <input type="text" v-model="newMovieYear"> <br>
      Director: <input type="text" v-model="newMovieDirector"> <br>
      Plot: <input type="text" v-model="newMoviePlot"> <br>
      English Film: <input type="text" v-model="newMovieEnglish"> <br>
      <button v-on:click="createMovie()">Add Movie</button>
    </div>
  
    <div v-for="movie in movies">
      <h3>Title: {{ movie.title }}</h3>
      <button v-on:click="showMovie(movie)">Movie Details</button>
    </div>

    <dialog id="movie-details">
      <form method="dialog">
        <h1>Movie Details</h1>
        <p>Title: <input type="text" v-model="currentMovie.title"></p>
        <p>Year: <input type="text" v-model="currentMovie.year"></p>
        <p>Director: <input type="text" v-model="currentMovie.director"></p>
        <p>Plot: <input type="text" v-model="currentMovie.plot"></p>
        <p>English Film: <input type="text" v-model="currentMovie.english"></p>
        <button v-on:click="updateMovie(currentMovie)">Update</button>
        <button v-on:click="destroyMovie(currentMovie)">Delete</button>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      movies: [],
      errors: [],
      currentMovie: {},
      newMovieTitle: "",
      newMovieYear: "",
      newMovieDirector: "",
      newMoviePlot: "",
      newMovieEnglish: "",
      message: "Welcome to The Movie App!"
    };
  },
  created: function() {
    this.indexMovies();
  },
  methods: {
    indexMovies: function() {
      axios.get("/api/movies").then(response => {
        console.log("All Movies", response.data);
        this.movies = response.data;
        console.log(this.movies);
      });
    },
    createMovie: function() {
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        director: this.newMovieDirector,
        plot: this.newMoviePlot,
        english: this.newMovieEnglish
      };

      axios
        .post("/api/movies", params)
        .then(response => {
          console.log("Successfully created new movie.", response.data);
          this.movies.push(response.data);
          this.newMovieTitle = "";
          this.newMovieYear = "";
          this.newMovieDirector = "";
          this.newMoviePlot = "";
          this.newMovieEnglish = "";
        })
        .catch(error => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },
    showMovie: function(movie) {
      console.log(movie);
      this.currentMovie = movie;
      document.querySelector("#movie-details").showModal();
    },
    updateMovie: function(movie) {
      var updateParams = {
        title: movie.title,
        year: movie.year,
        director: movie.director,
        plot: movie.plot,
        english: movie.english
      };

      axios
        .patch(`/api/movies/${movie.id}`, updateParams)
        .then(response => {
          console.log("Successfully updated movie", response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
        });
    },
    destroyMovie: function(movie) {
      axios.delete(`/api/movies/${movie.id}`).then(respone => {
        console.log("Successfully deleted movie", respone.data);
        var index = this.movies.indexOf(movie);
        this.movies.splice(index, 1);
      });
    }
  }
};
</script>