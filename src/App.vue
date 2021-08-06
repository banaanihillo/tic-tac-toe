<template>
<div id="app">
  <main v-if="!player">
    <h2> Select your player </h2>
    <button @click="selectPlayer('X')">X</button>
    <button @click="selectPlayer('O')">O</button>
  </main>

  <main v-else class="tic-tac-toe">
    <span v-for="tile in tiles" :key="tile.id">
      <button @click="fillTile(tile.id)" :disabled="tile.disabled">
        {{tile.value}}
      </button>
    </span>
  </main>
  <p v-if="gameOver">
    The game is over
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
      gameOver: false
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
    },
    selectPlayer(player) {
      this.player = player
      this.opposingPlayer = (
        player === "X"
          ? "O"
          : "X"
      )
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
