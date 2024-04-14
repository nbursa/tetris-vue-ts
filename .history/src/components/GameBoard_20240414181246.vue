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
import { defineComponent, ref, onMounted, onUnmounted, nextTick } from 'vue';

export default defineComponent({
  name: 'GameBoard',
  setup() {
    const columns = ref(Array.from({ length: 10 }, () => Array(20).fill(0)));

    const blockPosition = ref({ x: 4, y: 0 });

    // let gameLoopId: number | undefined;

    const lastMoveTime = ref(Date.now());
    const moveInterval = 500;

    const willCollide = (x: number, y: number) => {
      return y >= columns.value[x].length || columns.value[x][y] !== 0;
    };

    const placeBlock = () => {
      columns.value[blockPosition.value.x][blockPosition.value.y] = 1;
      columns.value[blockPosition.value.x] = [...columns.value[blockPosition.value.x]];
    };

    const resetBlock = async () => {
      blockPosition.value = { x: 4, y: 0 };
      lastMoveTime.value = Date.now();
      if (willCollide(blockPosition.value.x, blockPosition.value.y)) {
        alert('Game Over!');
      } else {
        nextFrame();  // Continue animation with a new block
      }
    };

    const moveBlock = () => {
      const currentTime = Date.now();
      if (currentTime - lastMoveTime.value >= moveInterval) {
        if (!willCollide(blockPosition.value.x, blockPosition.value.y + 1)) {
          blockPosition.value.y += 1;
          lastMoveTime.value = currentTime;
          console.log("blockPosition.value.y:", blockPosition.value.y)
        } else {
          placeBlock();
          resetBlock();
        }
      }
      nextFrame();

    };

    const nextFrame = () => {
      requestAnimationFrame(moveBlock);
    };

    onMounted(() => {
      nextFrame();
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
