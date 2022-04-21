<template>
  <b-container>
    <b-nav>
      <b-nav-item><router-link to="/">Home</router-link></b-nav-item> 
      <b-nav-item><router-link to="/pressupost">Pressupost</router-link></b-nav-item>
    </b-nav>
    <router-view/>
  </b-container>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

ul.nav {
  padding: 30px;
}

ul.nav a {
  font-weight: bold;
  color: #2c3e50;
  text-decoration: none;
}

ul.nav a.router-link-exact-active {
  color: #1ba8ab !important;
  text-decoration: underline;
}

</style>
