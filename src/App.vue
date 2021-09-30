<template>
  <div class="app">
    <img class="logo" alt="Vue logo" src="./assets/logo.png">
    
    <Board />
  </div>
</template>

<script>
import Board from './components/Board';

export default {
  name: 'App',
  components: {
    Board
  }
}
</script>

<style>
.app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  
  user-select: none; /* supported by Chrome and Opera */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
}

.logo {
  width: 100px;
}
</style>
