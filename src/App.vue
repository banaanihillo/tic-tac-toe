<template>
<div id="app">
  <main v-if="!player">
    <h2> Select your player </h2>
    <button @click="selectPlayer('X')">X</button>
    <button @click="selectPlayer('O')">O</button>
  </main>

  <main v-else class="tic-tac-toe">
    <span v-for="tile in tiles" :key="tile.id">
      <button
        @click="fillTile(tile.id)"
        :disabled="tile.disabled || gameOver"
      >
        {{tile.value}}
      </button>
    </span>
  </main>
  <p v-if="gameOver">
    The game is over. <br />
    {{winner ? `${winner} won!` : "It's a tie!"}}
  </p>
</div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      player: null,
      opposingPlayer: null,
      tiles: [...Array(9)].map((_value, index) => {
        return {
          id: index,
          disabled: false,
          value: ""
        }
      }),
      gameOver: false,
      winner: null
    }
  },
  methods: {
    fillTile(tileID) {
      this.tiles[tileID].value = this.player
      this.tiles[tileID].disabled = true

      const remainingTiles = this.tiles.filter((tile) => {
        return tile.value === ""
      }).map((tile) => tile.id)

      if (remainingTiles.length === 0) {
        return this.gameOver = true
      }

      const randomTileIndex = Math.floor(
        Math.random() * remainingTiles.length
      )
      const randomTile = remainingTiles[randomTileIndex]
      this.tiles[randomTile].value = this.opposingPlayer
      this.tiles[randomTile].disabled = true

      if (remainingTiles.length < 5) {
        if (this.isWinner(this.player)) {
          this.gameOver = true
          this.winner = this.player
          return
        }
        if (this.isWinner(this.opposingPlayer)) {
          this.gameOver = true
          this.winner = this.opposingPlayer
          return
        }
      }
    },
    selectPlayer(player) {
      this.player = player
      this.opposingPlayer = (
        player === "X"
          ? "O"
          : "X"
      )
    },
    isWinner(player) {
      for (let index = 0; index < this.tiles.length; index++) {
        if (
          this.checkRows(player)
          || this.checkColumn(player)
          || this.checkTopRight(player)
          || this.checkBottomRight(player)
        ) {
          return true
        }
      }
    },
    checkRows(player) {
      const rowOne = this.checkWinner(this.tiles.slice(0, 3), player)
      const rowTwo = this.checkWinner(this.tiles.slice(3, 6), player)
      const rowThree = this.checkWinner(this.tiles.slice(6, 9), player)
      if (rowOne || rowTwo || rowThree) {
        return true
      } else {
        return false
      }
    },
    checkColumn(player) {
      const columnOne = this.checkWinner(
        [this.tiles[0], this.tiles[3], this.tiles[6]],
        player
      )
      const columnTwo = this.checkWinner(
        [this.tiles[1], this.tiles[4], this.tiles[7]],
        player
      )
      const columnThree = this.checkWinner(
        [this.tiles[2], this.tiles[5], this.tiles[8]],
        player
      )
      if (columnOne || columnTwo || columnThree) {
        return true
      } else {
        return false
      }
    },
    checkTopRight(player) {
      const diagonallyTopToRight = this.checkWinner(
        [this.tiles[0], this.tiles[4], this.tiles[8]],
        player
      )
        
      if (diagonallyTopToRight) {
        return true
      } else {
        return false
      }
    },
    checkBottomRight(player) {
      const diagonallyBottomToRight = this.checkWinner(
        [this.tiles[2], this.tiles[4], this.tiles[6]],
        player
      )
        
      if (diagonallyBottomToRight) {
        return true
      } else {
        return false
      }
    },
    checkWinner(tiles, player) {
      if (tiles.every(tile => tile.value === player)) {
        return true
      } else {
        return false
      }
    }
  }
}
</script>

<style>
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  background-color: black;
  text-align: center;
  color: magenta;
  margin-top: 60px;
}

button {
  background-color: plum;
  width: 5em;
  height: 5em;
}

button:focus,
button:hover {
  background-color: violet;
}

button[disabled] {
  background-color: magenta;
  color: black;
}

.tic-tac-toe {
  width: min-content;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
}
</style>
