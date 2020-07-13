<template>
  <div id="app">

    <div id="navigation">
      <ul>
        <li><router-link to="/">Home</router-link></li>
        <li><router-link to="/about">About</router-link></li>
        <li><router-link to="/movies">All Movies</router-link></li>
        <li><router-link to="/movies/new">New Movie</router-link></li>
        <span v-if="isLoggedIn()">
          <li><router-link to="/logout">Logout</router-link></li>
        </span>
        <span v-else>
          <li><router-link to="/signup">Signup</router-link></li>
          <li><router-link to="/login">Login</router-link></li>
        </span>
      </ul>
    </div>
    
    <router-view/>

    <div id="footer">
    <p class="lf">Copyright &copy; 2010 <a href="#">SiteName</a> - All Rights Reserved</p>
    <p class="rf">Design by <a href="http://chocotemplates.com/">ChocoTemplates.com</a></p>
    <div style="clear:both;"></div>

  </div>

  </div>
  
</template>

<style>
</style>

<script>
export default {
  data: function() {
    return {
      flashMessage: ""
    };
  },
  methods: {
    isLoggedIn() {
      return localStorage.getItem("jwt");
    },
    getUserId() {
      return localStorage.getItem("user_id");
    }
  }
};
</script>
