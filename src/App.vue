<template>
  <div id="app">
    <img alt="tic-tac-logo" src="./assets/logo.png" />
    <Tictac />
  </div>
</template>

<script>
import Tictac from "./components/Tictac.vue";

export default {
  name: "App",
  components: {
    Tictac
  }
};
</script>

<style>
#app {
  font-family: monospace;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
