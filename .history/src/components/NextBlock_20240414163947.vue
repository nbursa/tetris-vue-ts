<template>
  <div class="next-block">
    <!-- Preview of next block -->
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'NextBlock',
  // Add any props, data, or methods needed here
});
</script>

<style scoped>
.next-block {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-template-rows: repeat(4, 1fr);
  gap: 2px;
}
</style>
