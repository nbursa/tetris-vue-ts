<template>
  <div class="game-board">
    <div v-for="(column, columnIndex) in columns" :key="'column-' + columnIndex" class="column">
      <div v-for="(_, rowIndex) in column" :key="'cell-' + columnIndex + '-' + rowIndex"
        :class="{ 'filled': columns[columnIndex][rowIndex] !== 0 }" class="cell">
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, onUnmounted, watch } from 'vue';

export default defineComponent({
  name: 'GameBoard',
  setup() {
    const columns = ref(Array.from({ length: 10 }, () => Array(20).fill(0)));

    const blockPosition = ref({ x: Math.floor(Math.random() * 10), y: 0 });

    let gameLoopId: number | undefined;

    const willCollide = (x: number, y: number) => {
      return y >= columns.value[x].length || columns.value[x][y] !== 0;
    };

    const placeBlock = () => {
      columns.value[blockPosition.value.x][blockPosition.value.y] = 1;
      columns.value = [...columns.value]; // Ensure reactivity by updating the reference
    };

    const moveBlock = () => {
  if (!willCollide(blockPosition.value.x, blockPosition.value.y + 1)) {
    blockPosition.value.y += 1;
    // Explicitly trigger reactivity for Vue to notice the change
    columns.value[blockPosition.value.x] = [...columns.value[blockPosition.value.x]];
  } else {
    placeBlock();
    resetBlock();
  }
};

const placeBlock = () => {
  columns.value[blockPosition.value.x][blockPosition.value.y] = 1;
  // Trigger update for the specific column only
  columns.value[blockPosition.value.x] = [...columns.value[blockPosition.value.x]];
};

    const resetBlock = () => {
      blockPosition.value = { x: Math.floor(Math.random() * 10), y: 0 };
      if (willCollide(blockPosition.value.x, blockPosition.value.y)) {
        alert('Game Over!');
        clearInterval(gameLoopId);
      }
    };

    onMounted(() => {
      gameLoopId = setInterval(moveBlock, 500);
    });

    onUnmounted(() => {
      if (gameLoopId) clearInterval(gameLoopId);
    });

    return { columns };
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
