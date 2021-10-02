<template>
  <div class="board">
    <BoardItem
      v-for="(cell, index) in cells"
      :key="cell + '-' + index"
      :icon="cell"
      @mousedown="mousedown(index)"
      @mouseup="mouseup(index)"
      @mousemove="go(index)"
      :selected="checkRoad(index)"
      :closed="checkClosedRoad(index)"
    />
  </div>
</template>

<script>
import BoardItem from "@/components/BoardItem";
import { ref } from 'vue';

export default {
  name: 'Board',
  
  components: {
    BoardItem
  },
  
  setup() {
    const cells = ref([3, 1, 0, 1, 0, 0, 0, 0, 2, 0, 0, 0, 2, 0, 0, 3]);
    const path = ref([]);
    const size = ref(4);
    const closedPath = ref([]);
    
    const mousedown = (index) => {
      path.value = [];
      
      if (cells.value[index]) {
        path.value.push(index);
      }
    }
    
    const mouseup = (index) => {
      if (index !== path.value[0] && cells.value[index] === cells.value[path.value[0]]) {
        closedPath.value = closedPath.value.concat(path.value);
      }
      
      path.value = [];
    }
    
    const go = (index) => {
      if (path.value.length) {
        const lastIndex = path.value[path.value.length - 1];
        
        if (Math.abs(lastIndex - index) === 1 || Math.abs(lastIndex - index) === size.value) {
          path.value.push(index);
        }
      }
    }
    
    const checkRoad = (index) => {
      return path.value.findIndex(p => p === index) > -1;
    }
    
    const checkClosedRoad = (index) => {
      return closedPath.value.findIndex(p => p === index) > -1;
    }

    return {
      cells,
      mousedown,
      mouseup,
      go,
      path,
      checkRoad,
      checkClosedRoad,
    }
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
