<template>
  <div class="game">
    <h1>Tic Tac Toe</h1>
    <div class="board">
      <div class="row">
        <div 
          v-for="(cell, index) in board.slice(0, 3)" 
          :key="index"
          class="cell"
          @click="makeMove(index)"
        >{{ cell }}</div>
      </div>
      <div class="row">
        <div 
          v-for="(cell, index) in board.slice(3, 6)" 
          :key="index + 3"
          class="cell"
          @click="makeMove(index + 3)"
        >{{ cell }}</div>
      </div>
      <div class="row">
        <div 
          v-for="(cell, index) in board.slice(6, 9)" 
          :key="index + 6"
          class="cell"
          @click="makeMove(index + 6)"
        >{{ cell }}</div>
      </div>
    </div>
    <div class="status">
      {{ status }}
    </div>
    <button @click="resetGame" class="reset-button">
      New Game
    </button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const board = ref(Array(9).fill(''));
const isXNext = ref(true);
const winner = ref(null);

const calculateWinner = (squares) => {
  const lines = [
    [0, 1, 2], [3, 4, 5], [6, 7, 8], // rows
    [0, 3, 6], [1, 4, 7], [2, 5, 8], // columns
    [0, 4, 8], [2, 4, 6] // diagonals
  ];

  for (const [a, b, c] of lines) {
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  
  return null;
};

const status = computed(() => {
  if (winner.value) {
    return `Winner: ${winner.value}`;
  } else if (board.value.every(cell => cell !== '')) {
    return 'Game ended in a draw!';
  } else {
    return `Next player: ${isXNext.value ? 'X' : 'O'}`;
  }
});

const makeMove = (index) => {
  if (board.value[index] !== '' || winner.value) return;
  
  const newBoard = [...board.value];
  newBoard[index] = isXNext.value ? 'X' : 'O';
  board.value = newBoard;
  
  
  winner.value = calculateWinner(newBoard);
  

  if (!winner.value) {
    isXNext.value = !isXNext.value;
  }
};

const resetGame = () => {
  board.value = Array(9).fill('');
  isXNext.value = true;
  winner.value = null;
};
</script>

<style scoped>
body {
  margin: 0;
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background-color: #f5f5f5;
}

.game {
  text-align: center;
}

h1 {
  color: #2c3e50;
  margin-bottom: 1rem;
}

.status {
  margin: 1.5rem 0;
  font-size: 1.4rem;
  font-weight: bold;
  color: #2c3e50;
  min-height: 2rem;
}

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

.cell {
  width: 80px;
  height: 80px;
  border: 2px solid #2c3e50;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2.5rem;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.2s;
}

.cell:hover {
  background-color: #f0f0f0;
}

.reset-button {
  margin-top: 1rem;
  padding: 0.5rem 1.5rem;
  font-size: 1rem;
  background-color: #2c3e50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.2s;
}

.reset-button:hover {
  background-color: #1a252f;
}
</style>