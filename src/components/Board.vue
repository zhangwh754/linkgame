<template>
  <div class="board" @mouseleave="clear">
    <board-item  
      v-for="(item, index) in cells" 
      :key="item-index" 
      :icon="item" 
      @mousedown="mousedown(index)"
      @mouseup="mouseup(index)"
      :isselected="selected(index)"
      @mousemove="go(index)"
      :isClosed="checkClose(index)"
    />
  </div>

  <div @click="reload" class="reloadText"><i>重开</i></div>
</template>
<script>
import { ref } from 'vue'
import BoardItem from './BoardItem.vue'
export default {
  name: 'Board',
  components: {
    BoardItem
  },
  setup() {
    const cells = ref([])
    // path存储路径上的索引，第一位是按下的
    const path = ref([])
    const size = ref(4)
    const closedPath = ref([])
    let level = 1

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

    start(level)

    const reload = (level) => {start(level)}

    const mousedown = (index) => {
      // 清空path
      path.value = []
      // 把当前按下的索引放入path  不能从关闭的开始
      if(cells.value[index] && !checkClose(index)) {
        path.value.push(index)
      }
    }
    const mouseup = (index) => {
      // 如果弹起时索引和按下时索引不同，弹起的值和按下时的值相同，就输出ok
      if(index !== path.value[0] && cells.value[index] === cells.value[path.value[0]]) {
        closedPath.value = closedPath.value.concat(path.value)
        checkLvl()
      }
      // 弹起时必定清空path，检查一遍是否完成
      path.value = []
    }

    const selected = (index) => {
      // 如果当前按下的索引是path里面的，就返回true，让这个格子变色
      return path.value.findIndex(p => p === index) > -1
    }

    const go = (index) => {
      // 当path不为空的时候
      if(path.value.length) {
        // 当前索引为path里面最后一位
        const lastIndex = path.value[path.value.length - 1]

        //相当于移动到了相邻的格子            或者？？
        if(Math.abs(lastIndex - index) === 1 || Math.abs(lastIndex - index) === size.value && !checkClose(index)) {
          // 把当前格子索引推入path中
          path.value.push(index)
        }
        if (checkClose(index)
          || (cells.value[index] && cells.value[index] !== cells.value[path.value[0]])) {
          path.value = [];
        }
      }
    }

    // 鼠标移出连连看整个界面,立即判断一次
    const clear = () => {
      mouseup(path.value[path.value.length - 1])
    }

    const checkClose = (index) => {
      return closedPath.value.findIndex(p => p === index) > -1
    }

    const checkLvl = () => {
      let completed = true;
      
      cells.value.forEach((cell, index) => {
        if (cell && !checkClose(index)) {
          completed = false;
        }
      })
      
      if (completed) {
        alert('恭喜！你赢了！');
      }
    }

    return {
      cells, path,
      size,
      closedPath,
      mousedown, mouseup,
      selected,
      go,
      clear,
      checkClose,
      reload,
      checkLvl
    }
  }
}
</script>
<style lang="scss" scoped>
  .board {
    width: 200px;
    display: flex;
    flex-wrap: wrap;
    user-select: none;
    margin: 30px auto;
  }

  .reloadText {
    text-decoration: underline;
    text-align: center;
    cursor: pointer;
  }

  .reloadText:hover {
    color: red;
    transform: scale(1.2);
  }
</style>