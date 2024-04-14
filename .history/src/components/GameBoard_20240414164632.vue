<template>
  <div class="game-board">
    <!-- Iterate over each row and cell in the board -->
    <div v-for="(row, rowIndex) in board" :key="'row-' + rowIndex" class="row">
      <div v-for="(cell, cellIndex) in row" :key="'cell-' + rowIndex + '-' + cellIndex" :class="{ 'filled': cell }"
        class="cell">
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  name: 'GameBoard',
  setup() {
    // Example: 20 rows by 10 columns grid, initially all false (empty)
    const board = ref(Array.from({ length: 20 }, () => Array(10).fill(false)));

    // Methods to update the board might be added here

    return {
      board,
    };
  },
});
</script>

<style scoped>
.game-board {
  display: grid;
  grid-template-columns: repeat(10, 30px);
  /* Width of each cell */
  grid-template-rows: repeat(20, 30px);
  /* Height of each cell */
  gap: 1px;
}

.cell {
  width: 30px;
  height: 30px;
  background-color: #ddd;
  /* Default empty cells color */
  border: 1px solid #eee;
  /* Slight border for cells */
}

.filled {
  background-color: #333;
  /* Filled cells color */
}
</style>
