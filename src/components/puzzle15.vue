<template>
  <div class="container">
    <transition-group name="list"
                      class="puzzle"
                      tag="div">
      <template v-for="(row, y) in puzzle">
        <div v-for="(cell, x) in row"
             :key="cell+0"
             @click="move(y, x)"
             :class="[{'empty-tile': cell === null}, {'green-tile': x+y*3+1 == cell}]"
             class="tile">
          {{cell}}
        </div>
      </template>
    </transition-group>
    <div class="finished" v-if="finished">
      <h2>Finished!</h2>
      <button @click="newGame">Play a new game</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Puzzle15",
  props: {
    msg: String
  },
  data() {
    return {
      puzzle: [[1, 2, 3], [4, 5, 6], [7, 8, null]],
      empty: [2, 2],
      finished: false,
    };
  },
  mounted() {
    this.newGame();
  },
  methods: {
    isResolved() {
      for (let i = 0; i < this.puzzle.length; i++) {
        for (let j = 0; j < this.puzzle[i].length; j++) {
          if(this.puzzle[i][j] !== j+i*3+1 && this.puzzle[i][j] !== null)
            return false;
        } 
      }
      return true;
    },
    newGame() {
      for (let i = 0; i < 50; i++) {
        this.randomStart();
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
      // console.log('swap');
      // right side
      if (x + 1 < 3 && this.puzzle[x + 1][y] === null) {
        this.updateTile(x, y, x+1, y);
        // bottom side
      } else if (y - 1 >= 0 && this.puzzle[x][y - 1] === null) {
        this.updateTile(x, y, x, y-1);
        // left side
      } else if (x - 1 >= 0 && this.puzzle[x - 1][y] === null) {
        this.updateTile(x, y, x-1, y);

        // top side
      } else if (y + 1 < 3 && this.puzzle[x][y + 1] === null) {
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
    randomStart() {
      const possibleSwaps = this.possibleSwap(...this.empty);
      const tileToSwap = this.randomTile(possibleSwaps);
      this.move(...tileToSwap);
      this.empty = [...tileToSwap];
    },
    possibleSwap(x, y) {
      const possibleSwaps = [];
      if (x + 1 < 3) {
        possibleSwaps.push([x + 1, y]);
      }
      if (y - 1 >= 0) {
        possibleSwaps.push([x, y - 1]);
      }
      if (x - 1 >= 0) {
        possibleSwaps.push([x - 1, y]);
      }
      if (y + 1 < 3) {
        possibleSwaps.push([x, y + 1]);
      }
      return possibleSwaps;
    },
    randomTile(tiles) {
      return tiles[Math.floor(Math.random() * tiles.length)];
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
  padding-top: $margin-size;
  padding-left: $margin-size;
  margin: 0 auto;
}

.puzzle {
  width: 100%;
  height: 100%;
  flex-wrap: wrap;
  display: inline-flex;
}
.tile {
  width: calc(#{$tile-size} - #{$margin-size});
  height: calc(#{$tile-size} - #{$margin-size});
  background-color: #bec7be;
  cursor: pointer;
  text-align: center;
  vertical-align: middle;
  margin-right: $margin-size;
  margin-bottom: $margin-size;
  font-size: 6vw;
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
