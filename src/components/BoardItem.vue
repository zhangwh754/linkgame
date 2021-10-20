<template>
  <div 
    class="board-item" 
    :class="{selected: isselected, closed: isClosed}">
    <i v-if="icon" :class="'icon icon-' + icon"></i>
  </div>
</template>
<script>
export default {
  name: 'BoardItem',
  props: {
    icon: {
      type: Number,
      required: false,
      default: 0
    },
    isselected: {
      type: Boolean,
      required: false,
      default: false
    },
    isClosed: {
      type: Boolean,
      required: false,
      default: false
    }
  }
}
</script>
<style lang="scss" scoped>
  .board-item {
    width: 48px;
    height: 48px;
    border: 1px solid red;
    position: relative;
  }

  .closed {
    background-color: orange;
  }

  .selected {
    background-color: #00ff00;
  }
  
.icon {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
}

  .icon-1,
  .icon-2,
  .icon-3 {
    background: url('../assets/icons.png');
    background-size: 150px 150px;
  }

  .icon-1 {
    background-position: 0 0;
  }

  .icon-2 {
    background-position: 50px 0;
  }

  .icon-3 {
    background-position: 100px 0;
  }
</style>