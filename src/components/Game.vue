<template>
  <GameHelper
    v-if="
      state.player1Name &&
      state.player2Name &&
      state.currentPlayer !== null &&
      !state.gameOver
    "
    :playerName="currentPlayerName"
  />

  <div class="game">
    <Scoreboard
      v-if="
        state.player1Name &&
        state.player2Name &&
        state.currentPlayer !== null &&
        !state.gameOver
      "
      :playerName="state.player1Name"
      :wins="state.player1Score"
      playerSide="left"
    />
    <PlayerInput v-if="!state.player1Name" @submitName="handleSubmit" />

    <PlayerInput
      v-if="state.player1Name && !state.player2Name"
      @submitName="handleSubmit"
    />

    <Board
      v-if="
        state.player1Name &&
        state.player2Name &&
        state.currentPlayer !== null &&
        !state.gameOver
      "
      :currentPlayerSymbol="currentPlayerSymbol"
      :gameKey="state.gameKey"
      :board="state.board"
      @switchPlayer="handleSwitchPlayer"
      @gameOver="handleGameOver"
      @updateBoard="handleUpdateBoard"
    />

    <Scoreboard
      v-if="
        state.player1Name &&
        state.player2Name &&
        state.currentPlayer !== null &&
        !state.gameOver
      "
      :playerName="state.player2Name"
      :wins="state.player2Score"
      playerSide="right"
    />
  </div>

  <GameResult
    v-if="state.gameOver"
    :winner="state.gameOver"
    @resetGame="resetGame"
  />
  <NewSessionButton
    v-if="
      state.player1Name &&
      state.player2Name &&
      state.currentPlayer !== null &&
      !state.gameOver
    "
    @newSession="newSession"
  />
</template>

<script setup lang="ts">
import { reactive, computed, watch, onMounted } from "vue";
import Board from "./Board.vue";
import PlayerInput from "./PlayerInput.vue";
import Scoreboard from "./Scoreboard.vue";
import GameHelper from "./GameHelper.vue";
import GameResult from "./GameResult.vue";
import NewSessionButton from "./NewSessionButton.vue";

const state = reactive({
  player1Name: "",
  player2Name: "",
  player1Score: 0,
  player2Score: 0,
  currentPlayer: null as number | null,
  gameOver: null as string | null,
  gameKey: 0,
  board: Array.from({ length: 3 }, () => Array(3).fill("")),
});

const handleUpdateBoard = (newBoard: string[][]) => {
  state.board = newBoard;
};

watch(
  () => state,
  (newState) => {
    localStorage.setItem("gameState", JSON.stringify(newState));
  },
  { deep: true }
);

onMounted(() => {
  const savedState = localStorage.getItem("gameState") ?? "{}";
  const parsedState = JSON.parse(savedState);
  Object.assign(state, parsedState);
  if ("board" in parsedState) {
    state.board = parsedState.board;
  } else {
    state.board = Array.from({ length: 3 }, () => Array(3).fill(""));
  }
});

const decideFirstPlayer = () => {
  return Math.random() < 0.5 ? 1 : 2;
};

const currentPlayerSymbol = computed(() => {
  return state.currentPlayer === 1 ? "X" : "O";
});

const currentPlayerName = computed(() => {
  return state.currentPlayer === 1 ? state.player1Name : state.player2Name;
});

watch(
  () => ({ p1: state.player1Name, p2: state.player2Name }),
  ({ p1, p2 }) => {
    if (p1 && p2 && state.currentPlayer === null) {
      state.currentPlayer = decideFirstPlayer();
    }
  },
  { immediate: true, deep: true }
);

const handleSubmit = (name: string) => {
  if (!state.player1Name) {
    state.player1Name = name;
  } else if (!state.player2Name) {
    state.player2Name = name;
  }
};

const handleSwitchPlayer = () => {
  state.currentPlayer = state.currentPlayer === 1 ? 2 : 1;
};

const handleGameOver = (result: string) => {
  let winnerName = null;

  if (result === "X") {
    state.player1Score++;
    winnerName = state.player1Name;
  } else if (result === "O") {
    state.player2Score++;
    winnerName = state.player2Name;
  }

  state.gameOver = winnerName || result;
};

const resetGame = () => {
  state.currentPlayer = decideFirstPlayer();
  state.gameOver = null;
  state.gameKey++;
  state.board = Array.from({ length: 3 }, () => Array(3).fill(""));
};

const newSession = () => {
  Object.assign(state, {
    player1Name: "",
    player2Name: "",
    player1Score: 0,
    player2Score: 0,
    currentPlayer: null,
    gameOver: null,
    gameKey: state.gameKey + 1,
    board: Array.from({ length: 3 }, () => Array(3).fill("")),
  });

  localStorage.removeItem("gameState");
};
</script>

<style scoped>
.game {
  display: flex;
  justify-content: space-between;
}
</style>
