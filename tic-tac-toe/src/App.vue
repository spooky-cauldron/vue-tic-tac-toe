<script setup lang="ts">
import { ref, provide } from "vue";
import Board from "./components/Board.vue";
import { changePlayerKey, currentPlayerKey } from "./keys";
import { playerX, playerO, players } from "./players";

const currentPlayer = ref(players[0]);
const changePlayer = () => {
  currentPlayer.value = currentPlayer.value === playerX ? playerO : playerX;
};
provide(currentPlayerKey, currentPlayer);
provide(changePlayerKey, changePlayer);
</script>

<template>
  <Board />
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
