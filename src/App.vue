<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <board />
  </div>
</template>

<script>
import Board from './components/Board.vue'

export default {
  name: 'App',
  components: {
    Board,
  }
}
</script>

<style lang="scss">
  #app {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
</style>
