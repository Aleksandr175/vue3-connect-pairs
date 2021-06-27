<template>
  <div class="board">
    <BoardItem v-for="(cell, index) in cells" :key="cell + '-' + index" :icon="cell" />
  </div>
</template>

<script>
import BoardItem from "@/components/BoardItem";

export default {
  name: 'Board',
  
  components: {
    BoardItem
  },
  
  data() {
    return {
      cells: [3, 1, 0, 1, 0, 0, 0, 0, 2, 0, 0, 0, 2, 0, 0, 3],
    };
  }
}

</script>

<style>
.board {
  display: flex;
  flex-wrap: wrap;
  width: 200px;
  height: 200px;
  margin: 0 auto;
}
</style>
