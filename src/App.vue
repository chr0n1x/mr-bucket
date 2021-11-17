<template>
  <iframe width="560" height="315" src="https://www.youtube.com/embed/D_Bbf7hX5I0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
  <SocketBucket/>
</template>

<script>
import SocketBucket from './components/SocketBucket.vue'

export default {
  name: 'App',
  components: {
    SocketBucket
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
