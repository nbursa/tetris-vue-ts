<template>
  <div class="tetris-block"></div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

export default defineComponent({
  name: 'TetrisBlock',
  props: {
    color: {
      type: String as PropType<string>,
      required: true
    },
  },
});
</script>

<style scoped>
.tetris-block {
  width: 100%;
  height: 100%;
  background-color: var(--color, blue);
  /* Use CSS variable for dynamic coloring */
}
</style>
