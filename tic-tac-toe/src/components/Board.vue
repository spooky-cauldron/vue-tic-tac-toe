<script setup lang="ts">
import { ref } from "vue";

const gameState = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const currentPlayer = ref("X");

const getSquareOwner = (row: number, col: number): string =>
  gameState.value[row][col];

const isSquareSelectable = (row: number, col: number): boolean =>
  getSquareOwner(row, col) !== "";

const pickSquare = (row: number, col: number) => {
  gameState.value[row][col] = currentPlayer.value;
};
</script>

<template>
  <h2>Player {{ currentPlayer }}</h2>
  <table>
    <tr v-for="(row, rowIdx) in gameState">
      <td v-for="(item, colIdx) in row">
        <button
          class="board-btn"
          :disabled="isSquareSelectable(rowIdx, colIdx)"
          @click="pickSquare(rowIdx, colIdx)"
        >
          {{ getSquareOwner(rowIdx, colIdx) }}
        </button>
      </td>
    </tr>
  </table>
</template>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.board-btn {
  text-align: center;
  width: 5rem;
  height: 5rem;
}

.board-btn:hover:enabled {
  background-color: green;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
