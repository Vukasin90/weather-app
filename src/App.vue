<template>
  <weatherApp></weatherApp>
</template>

<script>
import weatherApp from './components/weatherApp.vue'

export default {
  name: 'App',
  components: {
    weatherApp
  }
}
</script>

<style>
body {
  margin: 0;
  overflow: hidden;
}
#app {
  font-family: 'montserrat', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  z-index: 2;
  overflow: hidden;
}
</style>
