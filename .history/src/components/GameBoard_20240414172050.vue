<template>
  <div class="game-board">
    <!-- Loop over columns first -->
    <div v-for="(column, columnIndex) in columns" :key="'column-' + columnIndex" class="column">
      <!-- Loop over each cell in the column -->
      <div v-for="(cell, rowIndex) in column" :key="'cell-' + columnIndex + '-' + rowIndex"
        :class="{ 'filled': isBlock(columnIndex, rowIndex) }" class="cell">
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, onUnmounted } from 'vue';

export default defineComponent({
  name: 'GameBoard',
  setup() {
    // Initialize columns as an array of arrays (each sub-array is a column)
    const columns = ref(Array.from({ length: 10 }, () => Array(20).fill(false)));

    const blockPosition = ref({ x: 5, y: 0 });

    const isBlock = (columnIndex: number, rowIndex: number): boolean => {
      return columnIndex === blockPosition.value.x && rowIndex === blockPosition.value.y;
    };

    const moveBlock = () => {
      if (blockPosition.value.y < columns.value[0].length - 1) {
        blockPosition.value.y += 1; // Move block down in the column
      } else {
        blockPosition.value.y = 0; // Reset block to the top of the column
      }
    };

    let gameLoopId: number | undefined;
    onMounted(() => {
      gameLoopId = setInterval(moveBlock, 500);
    });

    onUnmounted(() => {
      if (gameLoopId) {
        clearInterval(gameLoopId);
      }
    });

    return { columns, isBlock };
  },
});
</script>

<style scoped>
.game-board {
  display: grid;
  grid-template-columns: repeat(10, 30px);
  /* Each cell in a column */
  grid-auto-rows: 30px;
  /* Auto row size */
  gap: 1px;
}

.cell {
  width: 30px;
  height: 30px;
  border: 1px solid #eee;
  background-color: #ddd;
}

.filled {
  background-color: #333;
}
</style>
