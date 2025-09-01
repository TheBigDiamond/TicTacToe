<template>
  <div class="game">
    <h1>Tic Tac Toe</h1>
    <Tabellone ref="tabellone" @move-made="onMoveMade" @game-won="onGameWon" />
    <div class="status">
      {{ status }}
    </div>
    <button @click="resetGame" class="reset-button">New Game</button>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";
import Tabellone from "./components/Tabellone.vue";

const tabellone = ref(null);
const winner = ref(null);
const isXNext = ref(true);

const status = computed(() => {
  if (winner.value === "X" || winner.value === "O") {
    return `Winner: ${winner.value}`;
  } else if (winner.value === "draw") {
    return "Game ended in a draw!";
  } else {
    return `Next player: ${isXNext.value ? "X" : "O"}`;
  }
});

const onMoveMade = ({ isXNext: nextPlayer }) => {
  isXNext.value = nextPlayer;
};

const onGameWon = (result) => {
  winner.value = result;
};

const resetGame = () => {
  if (tabellone.value) {
    tabellone.value.reset();
  }
  winner.value = null;
  isXNext.value = true;
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
  padding: 20px;
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
