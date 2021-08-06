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
</div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      player: null,
      tiles: [...Array(9)].map((_value, index) => {
        return {
          id: index,
          disabled: false,
          value: ""
        }
      })
    }
  },
  methods: {
    fillTile(tileID) {
      this.tiles[tileID].value = this.player
      this.tiles[tileID].disabled = true
    },
    selectPlayer(player) {
      this.player = player
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
