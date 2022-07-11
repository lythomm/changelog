<template>
  <div>
    <Changelog />
  </div>
</template>

<script>
import Changelog from './components/Changelog.vue'

export default {
  name: 'App',
  components: {
    Changelog
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
