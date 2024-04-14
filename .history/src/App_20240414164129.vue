<script setup lang="ts">
import GameBoard from './components/GameBoard.vue'
import NextBlock from './components/NextBlock.vue'
import ScoreBoard from './components/ScoreBoard.vue'
</script>

<template>
  <div>
    <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
    <div id="tetris-game">
      <GameBoard />
      <div id="sidebar">
        <NextBlock />
        <ScoreBoard />
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
