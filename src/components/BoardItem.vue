<template>
  <div class="board-item">
    {{ icon ? icon : '' }}
  </div>
</template>

<script>

export default {
  name: 'BoardItem',
  
  props: {
    icon: {
      type: Number,
      required: false,
      default: 0,
    }
  }
}

</script>

<style>
.board-item {
  border: 1px solid #2c3e50;
  width: 48px;
  height: 48px;
}
</style>
