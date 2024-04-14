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

    const isBlock = (columnIndex: number, rowIndex: number): boolean => {
      return columnIndex === blockPosition.value.x && rowIndex === blockPosition.value.y;
    };

    const moveBlock = () => {
      if (blockPosition.value.y < columns.value[0].length - 1) {
        blockPosition.value.y += 1;
      } else {
        blockPosition.value.y = 0;
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
