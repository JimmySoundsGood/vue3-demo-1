<template>
  <div>
    <div class="square" @click="onClick">
      <template v-if="flag"></template>
      <template v-else>
        {{ text() }}
      </template>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue'
const emits = defineEmits(['onClickHandler'])
const props = defineProps(['num', 'row', 'col'])
const flag = ref(true)
const isClicked = ref(false)
const myNum = ref(0)

const text = () => {
  return myNum.value % 2 === 0 ? 'X' : 'O'
}

const onClick = () => {
  if (isClicked.value == false) {
    isClicked.value = true
    flag.value = false
    myNum.value = props.num
    emits('onClickHandler', text())
  }
}
</script>

<style scoped lang="scss">
.square {
  border: 1px solid black;
  width: 100px;
  height: 100px;
  font-size: 3em;
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
