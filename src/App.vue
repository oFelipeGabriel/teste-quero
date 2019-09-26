<template>
  <div id="app">
    <Header />
    <Index />
    <Footer />
  </div>
</template>

<script>
import Index from './components/Index.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

export default {
  name: 'app',
  components: {
    Index,
    Header,
    Footer
  }
}
</script>

<style>
@import 'assets/fontawesome-free-5.11.2-web/css/all.css';
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
body{
  background-color: #FBFBFB;
  margin: 0;
}
.texto{
color: #1F2D30
}
</style>
