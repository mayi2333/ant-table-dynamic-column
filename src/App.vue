<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
  <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
  <TableDemo />
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import TableDemo from './components/TableDemo.vue'

export default {
  name: 'App',
  components: {
    // HelloWorld,
    TableDemo
  }
}
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
