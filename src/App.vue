<template>
  <div id="app" class="font-sans antialiased text-black">
    <div id="nav" class="bg-purple-500">
      <div class="container flex items-center justify-between py-6">
        <div>
          <a href="/">
            <img src="./assets/logo.svg" alt="Logo">
          </a>
        </div>

        <div class="text-white">
          <router-link to="/" class="hover:text-gray-300">Home</router-link> |
          <router-link to="/about" class="hover:text-gray-300">About</router-link>
        </div>

      </div>
    </div>
    <router-view/>
  </div>
</template>

<script src = "./assets/main.css">

export default {
   
}
</script>
