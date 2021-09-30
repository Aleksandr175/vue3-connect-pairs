<template>
  <div>Уровень: {{ lvl }}</div>
  <div class="board">
    <BoardItem
      v-for="(cell, index) in cells"
      :key="cell + '-' + index"
      :icon="cell"
      @mousedown="mousedown(index)"
      @mouseup="mouseup(index)"
      @mousemove="go(index)"
      :selected="checkRoad(index)"
      :closed="isRoadClosed(index)"
    />
  </div>
  
  <div @click="reload()" class="reload">Сбросить уровень</div>
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
    let cells = ref([3, 1, 0, 1, 0, 0, 0, 0, 2, 0, 0, 0, 2, 0, 0, 3]);
    const path = ref([]);
    const size = ref(4);
    const closedPath = ref([]);
    const lvl = 1;
    
    const mousedown = (index) => {
      path.value = [];
      
      if (cells.value[index] && !isRoadClosed(index)) {
        path.value.push(index);
      }
    }
    
    const start = (lvl) => {
      if (lvl === 1) {
        cells.value = [3, 1, 0, 1, 0, 0, 0, 0, 2, 0, 0, 0, 2, 0, 0, 3];
      }
      
      if (lvl === 2) {
        cells.value = [3, 1, 1, 0, 0, 0, 0, 0, 2, 0, 0, 0, 2, 0, 0, 3];
      }
  
      size.value = 4;
      path.value = [];
      closedPath.value = [];
    }
    
    start();
    
    const reload = () => {
      start(lvl);
    }
    
    const mouseup = (index) => {
      if (index !== path.value[0] && cells.value[index] === cells.value[path.value[0]]) {
        closedPath.value = closedPath.value.concat(path.value);
      }
      
      path.value = [];
  
      checkLvl();
    }
    
    const checkLvl = () => {
      let completed = true;
      
      cells.value.forEach((cell, index) => {
        if (cell && !isRoadClosed(index)) {
          completed = false;
        }
      })
      
      if (completed) {
        alert('Вы выиграли!');
      }
    }
    
    const go = (index) => {
      if (path.value.length) {
        const lastIndex = path.value[path.value.length - 1];
        
        if ((Math.abs(lastIndex - index) === 1 || Math.abs(lastIndex - index) === size.value) && !isRoadClosed(index)) {
          path.value.push(index);
        }
        
        if (isRoadClosed(index)
          || (cells.value[index] && cells.value[index] !== cells.value[path.value[0]])) {
          path.value = [];
        }
      }
    }
    
    const checkRoad = (index) => {
      return path.value.findIndex(p => p === index) > -1;
    }
    
    const isRoadClosed = (index) => {
      return closedPath.value.findIndex(p => p === index) > -1;
    }

    return {
      cells,
      mousedown,
      mouseup,
      go,
      path,
      checkRoad,
      isRoadClosed,
      lvl,
      reload,
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
  margin: 20px auto;
}

.reload {
  text-decoration: underline;
  cursor: pointer;
}

</style>
