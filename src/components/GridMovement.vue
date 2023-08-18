<template>
 <div class="grid h-screen place-items-center">
  <div>
    <div class="flex" v-for="(row, rowIndex) in grid" :key="`row-${rowIndex}`" :class="[isLastRow(rowIndex) ? 'mb-0' : 'mb-1 mr-0']">
      <div v-for="(column, colIndex) in row" :key="`col-${rowIndex}-${colIndex}`" :class="[isLastCol(colIndex) ? 'mr-0' : 'mr-1', isActiveSquare([rowIndex, colIndex]) ? 'bg-green-400' : 'bg-gray-100' ]" class="box grid place-items-center" :style="{width: boxSizePixels, height:boxSizePixels}" @click="activateSquare([rowIndex, colIndex])">
        <img v-if="column.icon" :src="require(`@/assets/images/${column.icon}.png`)" :alt="column.iconAlt">
      </div>
    </div>
  </div>
  <button @click="setupGrid">Reroll Dungeon</button>
 </div>
</template>

<script>
export default {
  name: 'GridMovement',
  props: {
    rows: Number,
    columns: Number,
    boxSize: Number,
  },
  data() {
    return {
      activeSquare: [],
      grid: []
    }
  },
  mounted() {
    this.setupGrid()
  },
  watch: {
    rows() {
      this.setupGrid()
    },
    columns() {
      this.setupGrid()
    }
  },
  methods: {
    setupGrid() {
      this.grid = []
      for(let i=0; i<this.rows; i++){
        this.grid.push([])
        for(let j=0; j<this.columns; j++) {
          this.grid[i].push({
            coordinates: {
              row: i+1,
              column: j+1,
            },
          })
        }
      }
      this.activeSquare = []
      this.placeEntrance()
      this.placeExit()
      this.placeItem('potion', 'a potion, it will heal you when picked up')
      this.placeItem('goblin', 'an goblin enemy, look out!')
    },
    getRandomNumberFromRange(min, max) {
      min = Math.ceil(min)
      max = Math.floor(max-1)
      return Math.floor(Math.random() * (max - min + 1) + min)
    },
    getRandomSquareLocation(minRow = 0, maxRow = this.rows, minCol = 0, maxCol = this.columns) {
      const rowIndex = this.getRandomNumberFromRange(minRow, maxRow)
      const columnIndex = this.getRandomNumberFromRange(minCol, maxCol)
      return [rowIndex, columnIndex]
    },
    placeEntrance() {
      const [row, col] = this.getRandomSquareLocation(0, this.rows, 0, 2)
      this.activeSquare = [row, col]
      this.grid[row][col].icon = 'entrance'
      this.grid[row][col].iconAlt = 'the dungeon entrance'
    },
    placeExit() {
      const [row, col] = this.getRandomSquareLocation(0, this.rows, this.columns-2)
      this.grid[row][col].icon = 'open-door'
      this.grid[row][col].iconAlt = 'an open wooden door, this is your goal'
    },
    placeItem(image, altText) {
      const [row, col] = this.getRandomSquareLocation(2, this.rows, 2, this.columns-2)
      this.grid[row][col].icon = image
      this.grid[row][col].iconAlt = altText
    },
    isLastCol(index) {
      return index >= this.columns
    },
    isLastRow(index) {
      return index >= this.rows
    },
    activateSquare(coordinateArray) {
      if(this.activeSquare.toString() != coordinateArray.toString()) {
        this.activeSquare = coordinateArray
        return
      }
      this.activeSquare = []
    },
    isActiveSquare(coordinateArray) {
      return this.activeSquare.toString() === coordinateArray.toString()
    }
  },
  computed: {
    boxSizePixels() {
      return `${this.boxSize}px`
    },
  },
}
</script>

<style scoped>
.box {
  border-radius:2px;
}
button {
  background: lightgrey;
  padding:10px;
  color: black;
}
</style>
