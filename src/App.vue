<template>
  <div id="app">
    <Header />
    <Main />
    <Footer />
  </div>
</template>

<script>

import Main from './main/Main'
import Header from './main/Header'
import Footer from './main/Footer'

export default {
  name: 'App',
  components: { 
    Main,
    Header,
    Footer
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
  background: #ccc;
}
</style>
