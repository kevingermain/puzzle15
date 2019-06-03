<template>
  <div id="app">
    <h1>Puzzle 15</h1>
    <puzzle15 :size="selectedSize" marginSize="5px" />

    <div class="select">
      <select name="slct" id="slct" v-model.number="selectedSize">
        <option selected disabled>Size of the puzzle</option>
          <option value="3">3</option>
          <option value="4">4</option>
          <option value="5">5</option>
          <option value="6">6</option>
          <option value="10">10</option>
          <option value="15">15</option>
      </select>
    </div>
  </div>
</template>

<script>
import puzzle15 from './components/puzzle15.vue'

export default {
  name: 'app',
  components: {
    puzzle15,
  },
  data() {
    return {
      selectedSize: 3,
    }
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

  select {
    -webkit-appearance: none;
    -moz-appearance: none;
    -ms-appearance: none;
    appearance: none;
    outline: 0;
    box-shadow: none;
    border: 0 !important;
    background: #2c3e50;
    background-image: none;
    font-size: 2em;
  }
  /* Remove IE arrow */
  select::-ms-expand {
    display: none;
  }
  /* Custom Select */
  .select {
    position: relative;
    display: flex;
    width: 20em;
    height: 3em;
    line-height: 3;
    background: #2c3e50;
    overflow: hidden;
    border-radius: .25em;
    margin: 20px auto 0;
  }
  select {
    flex: 1;
    padding: 0 .5em;
    color: #fff;
    cursor: pointer;
  }
  /* Arrow */
  .select::after {
    content: '\25BC';
    position: absolute;
    top: 0;
    right: 0;
    padding: 0 1em;
    background: #34495e;
    cursor: pointer;
    pointer-events: none;
    -webkit-transition: .25s all ease;
    -o-transition: .25s all ease;
    transition: .25s all ease;
  }
  /* Transition */
  .select:hover::after {
    color: #f39c12;
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