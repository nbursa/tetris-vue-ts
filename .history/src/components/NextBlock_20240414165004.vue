<template>
  <div class="next-block">
    <!-- Iterate over each cell in the preview grid -->
    <div v-for="(row, rowIndex) in blockLayout" :key="'next-row-' + rowIndex" class="block-row">
      <div v-for="(cell, cellIndex) in row" :key="'next-cell-' + rowIndex + '-' + cellIndex" :class="{ 'filled': cell }">
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed, PropType } from 'vue';
type BlockType = 'I' | 'O' | 'T'; 

export default defineComponent({
  name: 'NextBlock',
  props: {
    nextBlockType: {
      type: String as PropType<string>,
      required: true
    }
  },
  setup(props) {
    // Example of a block layout for visualization based on type
    const blockLayouts = {
      I: [[1, 1, 1, 1]],
      O: [[1, 1], [1, 1]],
      T: [[0, 1, 0], [1, 1, 1]],
      // Additional block types...
    };

    const blockLayout = computed(() => {
      // Fallback to an empty grid if the block type is unknown
      return blockLayouts[props.nextBlockType] || [[0, 0, 0, 0]];
    });

    return {
      blockLayout
    };
  },
});
</script>

<style scoped>
.next-block {
  display: grid;
  grid-template-columns: repeat(4, 20px);
  /* Width of each cell */
  grid-template-rows: repeat(4, 20px);
  /* Height of each cell */
  gap: 2px;
}

.block-row .filled {
  background-color: #444;
  /* Filled cell color */
}

.block-row div {
  width: 20px;
  height: 20px;
  background-color: #eee;
  /* Default empty cell color */
  border: 1px solid #ddd;
  /* Border for visualization */
}
</style>
