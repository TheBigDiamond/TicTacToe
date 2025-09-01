<template>
  <div class="board">
    <div class="row" v-for="(row, rowIndex) in rows" :key="rowIndex">
      <Cella
        v-for="(cell, cellIndex) in row"
        :key="cellIndex"
        :value="board[cell]"
        @cell-click="() => makeMove(cell)"
      />
    </div>
  </div>
</template>

<script setup>
import { ref, computed, defineEmits } from "vue";
import Cella from "./Cella.vue";

const emit = defineEmits(["move-made", "game-won"]);

const board = ref(Array(9).fill(""));
const isXNext = ref(true);
const winner = ref(null);

const rows = [
  [0, 1, 2],
  [3, 4, 5],
  [6, 7, 8],
];

const calculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8], // rows
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8], // columns
    [0, 4, 8],
    [2, 4, 6], // diagonals
  ];

  for (const [a, b, c] of lines) {
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }

  return null;
};

const makeMove = (index) => {
  if (board.value[index] !== "" || winner.value) return;

  const newBoard = [...board.value];
  newBoard[index] = isXNext.value ? "X" : "O";
  board.value = newBoard;

  emit("move-made", { board: newBoard, isXNext: !isXNext.value });

  const currentWinner = calculateWinner(newBoard);
  if (currentWinner) {
    winner.value = currentWinner;
    emit("game-won", currentWinner);
  } else if (!newBoard.includes("")) {
    emit("game-won", "draw");
  } else {
    isXNext.value = !isXNext.value;
  }
};

const reset = () => {
  board.value = Array(9).fill("");
  isXNext.value = true;
  winner.value = null;
};

defineExpose({
  reset,
});
</script>

<style scoped>
.board {
  display: inline-block;
  border: 3px solid #2c3e50;
  border-radius: 5px;
  background: white;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  margin: 0 auto;
}

.row {
  display: flex;
}
</style>
