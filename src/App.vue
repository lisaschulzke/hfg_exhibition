<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <Interface msg=""/>
  </div>
</template>

<script>
import Interface from './components/interface.vue'


export default {
  name: 'App',
  components: {
    Interface
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
