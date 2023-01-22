<script setup>
import { ref, computed, onMounted } from "vue";
//13:19
const wins = ref(0);
const losses = ref(0);
const draws = ref(0);

const choice = ref(null);
const computerChoice = ref(null);
const verdict = ref(null);

const outcome = {
  rock: {
    rock: "draw",
    paper: "loss",
    scissors: "wind",
  },
  paper: {
    rock: "win",
    paper: "draw",
    scissors: "loss",
  },
  scissors: {
    rock: "loss",
    paper: "wind",
    scissors: "draw",
  },
};

const winPercentage = computed(() => {
  const total = wins.value + draws.value + losses.value;
  return total ? (wins.value / total) * 100 : 0;
});

const play = (c) => {
  choice.value = c;
  const choices = ["rock", "paper", "scissors"];

  const random = Math.floor(Math.random() * choices.length);
  computerChoice.value = choices[random];

  const outcome = outcome[c][computerChoice.value];

  if (outcome === "win") {
    wins.value++;
    verdict.value = "Ganaste";
  } else if (outcome === "loss") {
    losses.value++;
    verdict.value = "Perdiste";
  } else {
    draws.value++;
    verdict.value = "Es un empate!";
  }

  saveGame();
};

const saveGame = () => {
  localStorage.setItem("wins", wins.value);
  localStorage.setItem("draws", draws.value);
  localStorage.setItem("losses", losses.value);
};

const loadGame = () => {
  wins.value = localStorage.getItem("wins");
  draws.value = localStorage.getItem("draws");
  losses.value = localStorage.getItem("losses");
};

const resetRound = () => {
  choice.value = null;
  computerChoice = null;
  verdict.value = null;
};

onMounted(() => {
  loadGame();
  window.addEventListener("keypress", (e) => {
    if (e.key === "r") {
      resetRound();
    }
  });
});
</script>

<template>
  <div class="bg-gray-700 text-white text-center min-h-screen flex flex-col">
    <header class="container mx-auto p-6">
      <h1 class="text-4xl font-bold">Piedra, Papel O Tijeras</h1>
    </header>

    <main class="container mx-auto p-6 flex-l">
      <div
        v-if="choice === null"
        class="flex items-center justify-center -mx-6"
      >
        <button
          @click="play('rock')"
          class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-pink-500"
        >
          <img src="./assets/RockIcon.svg" alt="Rock" class="w-full" />
        </button>

        <button
          @click="play('paper')"
          class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-green-500"
        >
          <img src="./assets/PaperIcon.svg" alt="paper" class="w-full" />
        </button>

        <button
          @click="play('scissors')"
          class="bg-white rounded-full shadow-lg w-64 p-12 mx-6 transition-colors duration-300 hover:bg-yellow-500"
        >
          <img src="./assets/ScissorsIcon.svg" alt="scissor" class="w-full" />
        </button>
      </div>
    </main>
  </div>
</template>
