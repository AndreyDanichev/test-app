<template>
  <div id="app">
    <board />
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
    Board: () => import('./components/C-Board')
  }
}
</script>

<style>
* {
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
body {
  margin: 0;
}
</style>
