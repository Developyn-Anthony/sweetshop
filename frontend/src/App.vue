<template>
  <div id="app">
    <Stock />
  </div>
</template>

<script>
import Stock from "./components/stock.vue"

export default {
  name: 'App',
  components: {
    Stock
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
