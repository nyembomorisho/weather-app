<template>
  <div id="app">
    <home-page />
  </div>

</template>

<script>
import HomePage from './views/HomePage.vue'

export default {
  name: 'App',
  components: {
    HomePage
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  margin-top: 20px;
  margin: 10px;
}
</style>
