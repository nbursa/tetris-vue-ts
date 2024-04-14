<template>
  <div class="game-board">
    <div v-for="(column, columnIndex) in columns" :key="'column-' + columnIndex" class="column">
      <div v-for="(_, rowIndex) in column" :key="'cell-' + columnIndex + '-' + rowIndex"
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
    const columns = ref(Array.from({ length: 10 }, () => Array(20).fill(false)));

    const blockPosition = ref({ x: 5, y: 0 });

    // Check if the block at the next position will collide
    const willCollide = (x: number, y: number) => {
      return y >= columns.value[x].length || columns.value[x][y] !== 0;
    };

    const placeBlock = () => {
      columns.value[blockPosition.value.x][blockPosition.value.y] = 1; // Mark the cell as filled
    };

    const isBlock = (columnIndex: number, rowIndex: number): boolean => {
      return columnIndex === blockPosition.value.x && rowIndex === blockPosition.value.y;
    };

    const moveBlock = () => {
      if (!willCollide(blockPosition.value.x, blockPosition.value.y + 1)) {
        blockPosition.value.y += 1;
      } else {
        placeBlock(); // Place the block if it will collide
        resetBlock(); // Start a new block from the top
      }
    };

    const resetBlock = () => {
      blockPosition.value = { x: Math.floor(Math.random() * 10), y: 0 }; // Randomize the starting column for variety
      if (willCollide(blockPosition.value.x, blockPosition.value.y)) {
        alert('Game Over!'); // End game if the new block starts in a filled position
        clearInterval(gameLoopId);
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
  grid-auto-rows: 30px;
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
