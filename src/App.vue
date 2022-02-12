<template>
  <div id="app">
    <Main />
  </div>
</template>

<script>

import Main from './main/Main'

export default {
  name: 'App',
  components: { 
    Main
   }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #000;
}
</style>
