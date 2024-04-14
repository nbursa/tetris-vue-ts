<script setup lang="ts">
import { ref } from 'vue';
import GameBoard from './components/GameBoard.vue'
import NextBlock, { BlockType } from './components/NextBlock.vue'
import ScoreBoard from './components/ScoreBoard.vue'

const nextBlockType = ref('I') as b;
const score = ref(0);
const level = ref(1);
</script>

<template>
  <div>
    <div id="tetris-game">
      <GameBoard />
      <div id="sidebar">
        <NextBlock :nextBlockType="nextBlockType" />
        <ScoreBoard :score="score" :level="level" />
      </div>
    </div>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}

.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}

.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

#tetris-game {
  display: flex;
  justify-content: space-around;
  align-items: start;
  padding: 20px;
}

#sidebar {
  display: flex;
  flex-direction: column;
  gap: 20px;
}
</style>
