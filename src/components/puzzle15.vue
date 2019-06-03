<template>
  <div class="container" :style="`padding-top: ${marginSize}; padding-left: ${marginSize};`">
    <transition-group name="list"
                      class="puzzle"
                      tag="div">
      <template v-for="(row, y) in puzzle">
        <tile v-for="(cell, x) in row"
             :key="cell+0"
             :number="cell"
             :marginSize="marginSize"
             :tileSize="100 / size + '%'"
             :placed="x+y*puzzle.length+1 == cell"
             @click.native="move(y, x)" />
      </template>
    </transition-group>
    <div class="finished" v-if="finished">
      <h2>Finished!</h2>
      <button @click="newGame">Play a new game</button>
    </div>
  </div>
</template>

<script>
import tile from './tile';

export default {
  name: "Puzzle15",
  components: {
    tile,
  },
  props: {
    size: {
      type: Number,
      required: false,
      default: 10,
    },    
    marginSize: {
      type: String,
      required: false,
      default: '10px',
    },
  },
  data() {
    return {
      puzzle: [],
      empty: [this.size-1, this.size-1],
      finished: false,
    };
  },
  created() {
    this.buildPuzzle();
  },
  mounted() {
    this.newGame();
  },
  methods: {
    buildPuzzle() {
      this.puzzle = [];
      this.empty = [this.size-1, this.size-1];
      for (let i = 0; i < this.size; i++) {
        this.puzzle.push([]);
        for (let j = 0; j < this.size; j++) {
          if(j === this.size-1 && i === this.size-1)
            this.puzzle[i].push(null);
          else 
            this.puzzle[i].push(j+i*this.size+1);
        }
      }
      this.$forceUpdate();
    },
    isResolved() {
      for (let i = 0; i < this.size; i++) {
        for (let j = 0; j < this.size; j++) {
          if(this.puzzle[i][j] !== j+i*this.size+1 && this.puzzle[i][j] !== null)
            return false;
        }
      }
      return true;
    },
    newGame() {
      for (let i = 0; i < 50*this.size; i++) {
        this.shuffle();
      }
      this.finished = false;
    },
    move(x, y) {
      this.swap(x, y);
      if(this.isResolved())
        this.finished = true;
      this.$forceUpdate();
    },
    swap(x, y) {
      // right side
      if (x + 1 < this.size && this.puzzle[x + 1][y] === null) {
        this.updateTile(x, y, x+1, y);
        // bottom side
      } else if (y - 1 >= 0 && this.puzzle[x][y - 1] === null) {
        this.updateTile(x, y, x, y-1);
        // left side
      } else if (x - 1 >= 0 && this.puzzle[x - 1][y] === null) {
        this.updateTile(x, y, x-1, y);
        // top side
      } else if (y + 1 < this.size && this.puzzle[x][y + 1] === null) {
        this.updateTile(x, y, x, y+1);
      }
    },
    updateTile(x, y, x2, y2) {
      const row = this.puzzle[x2];
      row[y2] = this.puzzle[x][y];
      this.$set(this.puzzle, this.puzzle[x], row);
      
      const row2 = this.puzzle[x];
      row2[y] = null;
      this.$set(this.puzzle, this.puzzle[x], row2);
    },
    shuffle() {
      const possibleSwaps = this.possibleSwap(...this.empty);
      const tileToSwap = this.randomTile(possibleSwaps);
      this.move(...tileToSwap);
      this.empty = [...tileToSwap];
    },
    possibleSwap(x, y) {
      const possibleSwaps = [];
      if (x + 1 < this.size) {
        possibleSwaps.push([x + 1, y]);
      }
      if (y - 1 >= 0) {
        possibleSwaps.push([x, y - 1]);
      }
      if (x - 1 >= 0) {
        possibleSwaps.push([x - 1, y]);
      }
      if (y + 1 < this.size) {
        possibleSwaps.push([x, y + 1]);
      }
      return possibleSwaps;
    },
    randomTile(tiles) {
      return tiles[Math.floor(Math.random() * tiles.length)];
    }
  },
  watch: {
    size() {
      this.buildPuzzle();
      this.newGame();
    }
  }
};
</script>
<style lang="scss">
$margin-size: 10px;
$number_of_tiles: 3;
$tile-size: 100 / $number_of_tiles + "%";

.container {
  position: relative;
  width: 50vw;
  height: 50vw;
  max-width: 480px;
  max-height: 480px;
  background-color: #ffffffb0;
  border: 15px solid #ff4900d1;
  border-radius: 15px;
  margin: 0 auto;
}

.puzzle {
  width: 100%;
  height: 100%;
  flex-wrap: wrap;
  display: inline-flex;
}
.tile {
  flex-grow: 0;
  background-color: #bec7be;
  cursor: pointer;
  text-align: center;
  vertical-align: middle;
  display: flex;
  align-items: center;
  justify-content: center;
}
.empty-tile {
  background-color: transparent;
}
.green-tile {
  background-color: #89bd89;
}
.finished {
  position: absolute;
  top: 0;
  width: 100%;
  height: 100%;
  background: #ffffffcc;
  left: 0;
  display: inline-flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  h2 {
    font-size: 3rem;
    margin: 0 0 15% 0;
  }
  button {
    position: relative;
    vertical-align: top;
    width: 75%;
    height: 60px;
    padding: 0;
    font-size: 22px;
    color: white;
    text-align: center;
    text-shadow: 0 1px 2px rgba(0, 0, 0, 0.25);
    background: #e67e22;
    border: 0;
    border-bottom: 2px solid #da751c;
    cursor: pointer;
    -webkit-box-shadow: inset 0 -2px #da751c;
    box-shadow: inset 0 -2px #da751c;
  }
  button:active {
    top: 1px;
    outline: none;
    -webkit-box-shadow: none;
    box-shadow: none;
  }
}  
.list-move {
  transition: transform 0.75s;
}
</style>
