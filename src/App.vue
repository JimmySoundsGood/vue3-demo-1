<template>
  <div class="container">
    <div>
      <div class="title-container">
        <div>
          <span
            >&nbsp;{{
              `当前第${num}手, ${num % 2 === 0 ? 'X方的回合 ' : 'O方的回合:'}`
            }}</span
          >
        </div>
        <div>
          <span><button @click="reset">重新开始</button></span>
        </div>
      </div>
      <div :class="won ? 'row-disabled' : 'row'">
        <Cell @onClickHandler="onClickHandler(0, $event)" :num="num"></Cell>
        <Cell @onClickHandler="onClickHandler(1, $event)" :num="num"></Cell>
        <Cell @onClickHandler="onClickHandler(2, $event)" :num="num"></Cell>
      </div>
      <div :class="won ? 'row-disabled' : 'row'">
        <Cell @onClickHandler="onClickHandler(3, $event)" :num="num"></Cell>
        <Cell @onClickHandler="onClickHandler(4, $event)" :num="num"></Cell>
        <Cell @onClickHandler="onClickHandler(5, $event)" :num="num"></Cell>
      </div>
      <div :class="won ? 'row-disabled' : 'row'">
        <Cell @onClickHandler="onClickHandler(6, $event)" :num="num"></Cell>
        <Cell @onClickHandler="onClickHandler(7, $event)" :num="num"></Cell>
        <Cell @onClickHandler="onClickHandler(8, $event)" :num="num"></Cell>
      </div>
      <hr />
      <span>状态记录：</span>
      <ul v-for="(item, index) in matrix" :key="index">
        <li>{{ item }}</li>
      </ul>
      <hr />
      <span>游戏状态: </span>
      <span v-if="!won">游戏进行中</span>
      <span v-else>游戏结束！{{`${won}方获胜!`}}</span>
    </div>
  </div>
</template>

<script setup>
import Cell from './views/Cell.vue'
import { ref, reactive, watch } from 'vue'
import { useRouter } from 'vue-router'

/*变量声明 */
const router = useRouter()
const num = ref(0)
const won = ref('')
const matrix = reactive([
  [0, 0, 0],
  [0, 0, 0],
  [0, 0, 0]
])

/* 函数方法 */
const onClickHandler = (i, text) => {
  console.log('随机点击')
  matrix[Math.floor(i / 3)][i % 3] = text
  num.value += 1
  checkStatus()
}

const checkStatus = () => {
  // 每row检查三个是否相同
  for (let i = 0; i < 3; i++) {
    if (
      matrix[i][0] !== 0 &&
      matrix[i][0] == matrix[i][1] &&
      matrix[i][1] == matrix[i][2]
    ) {
      won.value = matrix[i][0]
    }
  }
  // 每col检查是否相同
  for (let j = 0; j < 3; j++) {
    if (
      matrix[0][j] !== 0 &&
      matrix[0][j] == matrix[1][j] &&
      matrix[1][j] == matrix[2][j]
    ) {
      won.value = matrix[j][0]
    }
  }
  // 对角线检查是否相同
  if (
    matrix[0][0] !== 0 &&
    matrix[0][0] === matrix[1][1] &&
    matrix[1][1] === matrix[2][2]
  ) {
    won.value = matrix[0][0]
  }

  if (
    matrix[0][2] !== 0 &&
    matrix[0][2] === matrix[1][1] &&
    matrix[1][1] === matrix[2][0]
  ) {
    won.value = matrix[1][1]
  }
}

const reset = () => {
  router.go(0)
}
</script>

<style scoped lang="scss">
.row {
  display: flex;
}

.row-disabled {
  display: flex;
  pointer-events: none;
}

.container {
  display: flex;
  height: 100vh;
}

.container > div:first-child {
  margin: auto;
}

.title-container {
  display: flex;
  justify-content: space-between;
}
</style>
