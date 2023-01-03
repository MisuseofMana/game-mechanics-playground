<template>
  <GridMovement :rows="3" :columns="3" :boxSize="40"/>
</template>

<script>
import GridMovement from './components/GridMovement.vue'

export default {
  name: 'App',
  components: {
    GridMovement
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
}
</style>
