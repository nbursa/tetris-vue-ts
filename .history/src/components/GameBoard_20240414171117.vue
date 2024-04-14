<template>
  <div class="game-board">
    <div v-for="(row, rowIndex) in rows" :key="'row-' + rowIndex" class="row">
      <div v-for="(_, cellIndex) in row" :key="'cell-' + rowIndex + '-' + cellIndex"
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
    const blockPosition = ref({ x: 5, y: 0 }); // Start in the middle of the top row, x fixed, y increases.

    const isBlock = (rowIndex: number, cellIndex: number): boolean => {
      return rowIndex === blockPosition.value.y && cellIndex === blockPosition.value.x;
    };

    const moveBlock = () => {
      if (blockPosition.value.y < rows.value.length - 1) {
        blockPosition.value.y += 1; // Move block down
      } else {
        blockPosition.value.y = 0; // Reset block to top
      }
    };

    let gameLoopId: number | undefined;

    onMounted(() => {
      gameLoopId = setInterval(moveBlock, 500); // Start the game loop
    });

    onUnmounted(() => {
      if (gameLoopId) {
        clearInterval(gameLoopId); // Clear the interval on unmount
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
  grid-template-rows: repeat(20, 30px);
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
