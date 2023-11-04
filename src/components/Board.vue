<script setup>
import Cell from './Cell.vue';
import {ref} from "vue";
import GameProcess from "@/utils/gameProcess";

const emit = defineEmits(['end']);
const board = ref([
  ['', '', ''],
  ['', '', ''],
  ['', '', '']
]);

let locked = false;

function doMoveHandler(i, j, value) {
  if (locked) {
    return;
  }
  board.value[i][j] = value;


  if (GameProcess.checkWin(board.value, 'x')) {
    emit('end', 'user');
    endGame();
  } else {
    botMove();
  }

}

function botMove() {
  locked = true;
  setTimeout(() => {

    const botMoving = GameProcess.botMoving(board.value);

    if (botMoving) {
      board.value[botMoving.i][botMoving.j] = 'o';
      if (GameProcess.checkWin(board.value, 'o')) {
        emit('end', 'bot');
        endGame();
      }
    } else {
      emit('end', 'draw');
      endGame();
    }
    locked = false;
  }, 300)
}

function endGame() {
  setTimeout(() => {
    board.value = [
      ['', '', ''],
      ['', '', ''],
      ['', '', '']
    ]
  }, 300)
}
</script>

<template>

  <div class="board">

    <template v-for="(iValue, i) in board">
      <template v-for="(jValue, j) in iValue">
        <Cell :value="jValue" @do-move="(value) => doMoveHandler(i, j, value)"/>
      </template>
    </template>

  </div>

</template>


<style scoped>
.board {
  display: grid;
  grid-template-rows: repeat(3, 1fr);
  grid-template-columns: repeat(3, 1fr);
  border: 3px solid #d0d0d0;
}
</style>