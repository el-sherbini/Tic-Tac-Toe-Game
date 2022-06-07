<script setup>
import { ref, computed } from "vue";
const player = ref("X");
const board = ref([
  ["", "", ""],
  ["", "", ""],
  ["", "", ""],
]);

const CalculateWinner = (squares) => {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
};

const winner = computed(() => CalculateWinner(board.value.flat()));

const MakeMove = (x, y) => {
  if (winner.value || board.value[x][y] !== "") return;

  board.value[x][y] = player.value;
  player.value = player.value === "X" ? "O" : "X";
};

const ResetGame = () => {
  board.value = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""],
  ];
  player.value = "X";
};
</script>

<template>
  <main class="min-h-screen pt-8 text-center dark:bg-gray-800 dark:text-white">
    <h1 class="mb-8 text-3xl font-bold uppercase">Tic Tac Toe</h1>

    <h3 class="mb-4 text-xl">Player {{ player }}'s turn</h3>

    <div class="mb-8 flex flex-col items-center">
      <div v-for="(row, x) in board" :key="x" class="flex">
        <div
          v-for="(cell, y) in row"
          :key="y"
          @click="MakeMove(x, y)"
          :class="`material-icons-outlined flex h-32 w-32 cursor-pointer items-center justify-center border border-white text-5xl hover:bg-gray-700 ${
            cell === 'X' ? 'text-pink-500' : 'text-blue-500'
          }`"
        >
          {{ cell }}
        </div>
      </div>
    </div>

    <div class="text-center">
      <h2 v-if="winner" class="mb-8 text-6xl font-bold">
        Player '{{ winner }}' wins!
      </h2>
      <button
        @click="ResetGame"
        class="duration-400 rounded bg-red-500 px-4 py-2 font-bold uppercase hover:bg-red-600"
      >
        Reset
      </button>
    </div>
  </main>
</template>
