<script setup lang="ts">
import { changePlayerKey, currentPlayerKey } from "@/keys";
import { players } from "@/players";
import { ref, inject, computed, type Ref } from "vue";

type SquareID = [number, number]; // [rowIdx, colIdx]
interface WinData {
  winner: string;
  squares: SquareID[];
}

const gameState = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);
const boardSize = 3;

const currentPlayer = inject(currentPlayerKey) as Ref<string>;
const changePlayer = inject(changePlayerKey) as () => void;

const getSquareOwner = (row: number, col: number): string =>
  gameState.value[row][col];

const isSquareSelectable = (row: number, col: number): boolean =>
  getSquareOwner(row, col) !== "";

const pickSquare = (row: number, col: number): void => {
  gameState.value[row][col] = currentPlayer.value;
  changePlayer();
};

const checkWinner = (): WinData => {
  const checks: SquareID[][] = [];

  const diagonal1: SquareID[] = [];
  const diagonal2: SquareID[] = [];
  for (let i = 0; i < boardSize; i++) {
    const row: SquareID[] = [];
    const col: SquareID[] = [];
    for (let j = 0; j < boardSize; j++) {
      row.push([i, j]);
      col.push([j, i]);
    }
    checks.push(row);
    checks.push(col);
    diagonal1.push([i, i]);
    diagonal2.push([i, boardSize - 1 - i]);
  }
  checks.push(diagonal1);
  checks.push(diagonal2);

  for (const check of checks) {
    for (const player of players) {
      if (
        check.filter(([row, col]) => gameState.value[row][col] === player)
          .length >= boardSize
      ) {
        return { winner: player, squares: check };
      }
    }
  }
  return { winner: "", squares: [] };
};

const win = computed(checkWinner);

const isWinningButton = (row: number, col: number): boolean => {
  return win.value.squares.some(
    ([winRow, winCol]) => winRow === row && winCol === col
  );
};
</script>

<template>
  <h2 v-if="win.winner === ''">Player {{ currentPlayer }}</h2>
  <h2 v-else>Winner: {{ win.winner }}</h2>
  <table>
    <tr v-for="(row, rowIdx) in gameState">
      <td v-for="(item, colIdx) in row">
        <button
          class="board-btn"
          :class="[isWinningButton(rowIdx, colIdx) && 'win-btn']"
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

.win-btn:disabled {
  background-color: yellow;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}
</style>
