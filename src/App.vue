<script setup>
import Board from './components/Board.vue';
import Score from './components/Score.vue';
import {ref} from 'vue';

const score = ref({
  user: 0,
  bot: 0,
  draw: 0
})

function endHandler(winner) {
  switch (winner) {
    case 'user':
      score.value.user++;
      break;
    case 'bot':
      score.value.bot++;
      break;
    case 'draw':
      score.value.draw++;
      break;
  }
}
</script>

<template>
  <div class="title">Tik-Tak-Toe</div>
  <div class="game">
    <Board @end="endHandler"/>
    <Score :score="score"/>
  </div>
</template>

<style scoped>
.title {
  font-size: 40px;
  color: white;
  text-align: center;
  margin-top: 50px;
}

.game {
  margin: 50px auto;
  max-width: 324px;
}
</style>