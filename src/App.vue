<template>
  <div id="app">
    <miniGame />
  </div>
</template>

<script>
import miniGame from './components/miniGame.vue';

export default {
  components: {
    miniGame,
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  text-align: center;
  max-width: 800px;
  margin: 0 auto;
}
</style>
