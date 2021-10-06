<template>
  <div id="app">
    <MovieSearch msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
import MovieSearch from './components/MovieSearch.vue'

export default {
  name: 'App',
  components: {
    MovieSearch
  }
}
</script>

<style lang="scss">
@import "~@/assets/scss/vendors/bootstrap-vue/index";
@import '~bootstrap-vue/dist/bootstrap-vue.css';
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 60px 60px 60px 60px;
}
</style>
