<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
      <p class="bg-black text-white">Lorem ipsum dolor sit amet consectetur adipisicing elit. Amet magnam hic neque? Odit dolores dolorum adipisci minima, fugit esse ullam voluptatem eos. Voluptatum natus placeat exercitationem alias dolorum sit doloribus.</p>
    </div>
    <router-view/>
  </div>
</template>

<script>
import "./assets/main.css";
export default {
  
}
</script>
<style>

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
