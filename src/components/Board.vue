<template>
  <div class="board">
    <div v-for="(row, i) in board" :key="i" class="row">
      <Cell
        v-for="(cell, j) in row"
        :key="j"
        :value="cell"
        @click="() => handleCellClick(i, j)"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive, watch } from "vue";
import Cell from "./Cell.vue";

const props = defineProps({
  currentPlayerSymbol: {
    type: String,
    default: "",
  },
  gameKey: {
    type: Number,
    default: 0,
  },
  board: {
    type: Array as () => Array<Array<string>>,
    default: () => [
      ["", "", ""],
      ["", "", ""],
      ["", "", ""],
    ],
  },
});

const emit = defineEmits(["switchPlayer", "gameOver", "updateBoard"]);

watch(
  () => props.board,
  (newBoard) => {
    state.board = newBoard;
  },
  { deep: true }
);

let state = reactive({
  board: props.board,
});

const createInitialState = () => {
  return {
    board: [
      ["", "", ""],
      ["", "", ""],
      ["", "", ""],
    ],
  };
};

watch(
  () => props.gameKey,
  () => {
    state = reactive(createInitialState());
  },
  { immediate: true }
);

const checkWin = (board: string[][]) => {
  const winConditions = [
    [board[0][0], board[0][1], board[0][2]],
    [board[1][0], board[1][1], board[1][2]],
    [board[2][0], board[2][1], board[2][2]],
    [board[0][0], board[1][0], board[2][0]],
    [board[0][1], board[1][1], board[2][1]],
    [board[0][2], board[1][2], board[2][2]],
    [board[0][0], board[1][1], board[2][2]],
    [board[0][2], board[1][1], board[2][0]],
  ];

  for (let i = 0; i < winConditions.length; i++) {
    if (
      winConditions[i][0] !== "" &&
      winConditions[i][0] === winConditions[i][1] &&
      winConditions[i][1] === winConditions[i][2]
    ) {
      return winConditions[i][0];
    }
  }

  if (board.flat().filter((cell) => cell === "").length === 0) {
    return "draw";
  }

  return false;
};

const handleCellClick = (row: number, col: number) => {
  if (props.board[row][col] === "") {
    const newBoard = props.board.map((row) => [...row]);
    newBoard[row][col] = props.currentPlayerSymbol;

    const result = checkWin(newBoard);

    if (result) {
      emit("gameOver", result);
    } else if (result === false) {
      emit("switchPlayer");
    }

    emit("updateBoard", newBoard);
  }
};
</script>

<style scoped>
.board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-gap: 0px;
}
</style>
