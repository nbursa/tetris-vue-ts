<template>
  <div class="score-board">
    <p>Score: {{ score }}</p>
    <p>Level: {{ level }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref, PropType } from 'vue';

export default defineComponent({
  name: 'ScoreBoard',
  props: {
    // Define props for score and level
    score: {
      type: Number,
      required: true
    },
    level: {
      type: Number,
      required: true
    }
  },
});
</script>

<style scoped>
.score-board {
  padding: 10px;
  font-size: 20px;
  color: white;
  background: black;
  text-align: center;
}
</style>
