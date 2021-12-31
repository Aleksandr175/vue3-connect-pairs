<template>
  <div>Уровень: {{ lvl }}</div>
  <div class="win" v-if="gameStatus === 1">Вы выиграли! Поздравляем!</div>
  <div class="board" :style="{ width: 50 * size + 'px', height: 50 * size + 'px'}">
    <BoardItem
      v-for="(cell, index) in cells"
      :key="cell + '-' + index"
      :icon-id="cell"
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
    let lvl = ref(1);
    const maxLvl = 2;
    let gameStatus = ref(0); // 0 - play, 1 - win
    
    const mousedown = (index) => {
      path.value = [];
      
      if (cells.value[index] && !isRoadClosed(index)) {
        path.value.push(index);
      }
    }
    
    const start = (lvl) => {
      if (lvl === 1) {
        cells.value = [3, 1, 0, 1, 0, 0, 0, 0, 2, 0, 0, 0, 2, 0, 0, 3];
        size.value = 4;
      }
      
      if (lvl === 2) {
        cells.value = [0, 2, 0, 1, 0, 0, 0, 0, 1, 0, 0, 0, 2, 0, 0, 3, 0, 0, 0, 0, 0, 0, 3, 0, 0];
        size.value = 5;
      }
  
      path.value = [];
      closedPath.value = [];
    }
    
    start(lvl.value);
    
    const reload = () => {
      start(lvl.value);
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
        goToNextLvl();
      }
    }
    
    const goToNextLvl = () => {
      lvl.value += 1;
      gameStatus.value = 0;
      
      if (lvl.value > maxLvl) {
        lvl.value = 1;
        gameStatus.value = 1;
      }
      
      start(lvl.value);
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
      size,
      gameStatus
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

.win {
  font-weight: 700;
}

</style>
