<template>
  <div class="game-board">
    <!-- Iterate over each row and cell in the board -->
    <div v-for="(row, rowIndex) in rows" :key="'row-' + rowIndex" class="row">
      <div v-for="(cell, cellIndex) in row" :key="'cell-' + rowIndex + '-' + cellIndex"
        :class="{ 'filled': isBlock(rowIndex, cellIndex) }" class="cell">
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, onUnmounted } from 'vue';

export default defineComponent({
  name: 'GameBoard',
  setup() {
    const rows = ref(Array.from({ length: 20 }, () => Array(10).fill(false)));
    const blockPosition = ref({ x: 5, y: 0 }); // Start in the middle of the top row

    const isBlock = (rowIndex: number, cellIndex: number) => {
      return rowIndex === blockPosition.value.y && cellIndex === blockPosition.value.x;
    };

    const moveBlock = () => {
      if (blockPosition.value.y < 19) {
        blockPosition.value.y += 1; // Move block down
      } else {
        blockPosition.value.y = 0; // Reset block to top if it hits the bottom
      }
    };

    let gameLoopId: number | undefined; // Store the interval ID here

    onMounted(() => {
      gameLoopId = setInterval(moveBlock, 500); // Assign setInterval to a variable
    });

    onUnmounted(() => {
      if (gameLoopId !== undefined) {
        clearInterval(gameLoopId); // Use the stored interval ID to clear it
      }
    });

    return {
      rows,
      isBlock
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
  transition: background-color 0.5s;
  /* Smooth color transition */
}
</style>
