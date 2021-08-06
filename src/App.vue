<template>
  <div id="app">
    <span
      v-for="tile in tiles"
      :key="tile.id"
      :class="[tile.value === 'X' ? selectedClass : '']"
    >
      <button @click="fillTile(tile.id)">
        {{tiles[tile.id].value}}
      </button>
    </span>
  </div>
</template>

<script> //

export default {
  name: 'App',
  data() {
    return {
      player: "X",
      tiles: [...Array(9)].map((_value, index) => {
        return {
          id: index,
          disabled: false,
          value: ""
        }
      }),
      selectedClass: "selected"
    }
  },
  methods: {
    fillTile(tile) {
      console.log(tile)
      this.tiles[tile].value = this.player
      console.log(this.tiles)
      console.log(this.tiles[tile])
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

.selected button {
  background-color: violet;
}

#app {
  width: min-content;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
}
</style>
