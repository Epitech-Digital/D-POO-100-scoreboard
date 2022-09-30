<template>
  <ScoreBoard></ScoreBoard>
</template>

<script>
import ScoreBoard from "@/components/ScoreBoard";

export default {
  name: 'App',
  components: {
    ScoreBoard
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
  margin: auto;
  width: 100%;
  height: 100%;
  overflow-y: hidden;
  display: flex;
  overflow-x: initial;
}

body {
  margin: 0;
  padding: 0;
  height: 100vh;
  width: 100vw;
 background-image: linear-gradient(to right top, #051937, #0e2041, #17264b, #212d56, #2a3460);
  background-size: cover;
  background-position: center center;
  overflow: hidden;
}
</style>