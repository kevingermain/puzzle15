<template>
  <div id="app">
    <h1>Puzzle 15</h1>
    <puzzle15 />
  </div>
</template>

<script>
import puzzle15 from './components/puzzle15.vue'

export default {
  name: 'app',
  components: {
    puzzle15,
  }
}
</script>

<style lang="scss">
html {
  height: 100%;
}
body {
  background-color: #f74902;
  background-image: linear-gradient(315deg, #ffb191 0%, #d8a793 74%);
  font-size: 16px;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;

  h1 {
    font-size: 4rem;
  }
}

@media only screen and (max-width: 480px) {
  #app {
    h1 { 
      font-size: 2rem;
    }
    h2 { 
      font-size: 1.5rem;
    }
    .puzzle {
      border-width: 5px;
    }
  }
}
</style>