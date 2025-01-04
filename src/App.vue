<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">

    <HelloWorld msg="Welcome to Your Vue.js App"/>

    <!-- Use the NewsList component with a query -->
    <NewsList query="f" />
    <NewsList query="Technology" />
    <NewsList query="Business" />
  </div>
</template>

<script>
import NewsList from './components/NewsList.vue';

export default {
  name: 'App',
  components: {
    NewsList
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
