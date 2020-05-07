<template>
  <div id="app">
    <Reader/>
  </div>
</template>

<script>
import Reader from './components/Reader.vue'

export default {
  name: 'App',
  components: {
    Reader
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
