<script setup>
import {ref} from "vue";

let leftScore = ref(parseInt(localStorage.getItem('left_score')) || 0)
let rightScore = ref(parseInt(localStorage.getItem('right_score')) || 0)

let resetQuestion = ref(false)
let winner = ref(false)

if (leftScore.value >= 7 || rightScore.value >= 7) winner.value = true;

const incrementLeft = (value) => {
  if (winner.value) return;
  leftScore.value += value
  localStorage.setItem('left_score', leftScore.value.toString());
  if (leftScore.value >= 7) winner.value = true;
}

const incrementRight = (value) => {
  if (winner.value) return;
  rightScore.value += value;
  localStorage.setItem('right_score', rightScore.value.toString());
  if (rightScore.value >= 7) winner.value = true;
}

const resetCounter = () => {
  leftScore.value = 0;
  rightScore.value = 0;
  localStorage.setItem('left_score', leftScore.value.toString());
  localStorage.setItem('right_score', rightScore.value.toString());
  resetQuestion.value = false;
  winner.value = false;
}

</script>

<template>
  <main
      class="w-screen h-screen bg-gradient-to-br from-gray-800 to-gray-900 flex flex-col justify-center items-center font-lato select-none">
    <h1 class="text-white text-4xl">Schnapsen counter</h1>

    <div class="w-screen flex justify-center items-center space-x-32">

      <div class="flex flex-col justify-between space-y-6">
        <button
            class="button bg-gradient-to-br from-cyan-500 to-blue-500 hover:shadow-lg hover:shadow-blue-600/70"
            @click="incrementLeft(1)">+ 1
        </button>
        <button
            class="button bg-gradient-to-br from-violet-500 to-fuchsia-500 hover:shadow-lg hover:shadow-fuchsia-700/70"
            @click="incrementLeft(2)">+ 2
        </button>
        <button
            class="button bg-gradient-to-br from-red-500 to-amber-500 hover:shadow-lg hover:shadow-amber-700/70"
            @click="incrementLeft(3)">+ 3
        </button>
      </div>

      <div class="w-1/2 flex justify-evenly text-[16rem] text-white">
        <span class="w-full text-center">{{ leftScore }}</span>
        <span>-</span>
        <span class="w-full text-center">{{ rightScore }}</span>
      </div>

      <div class="flex flex-col justify-between space-y-6">
        <button
            class="button bg-gradient-to-br from-cyan-500 to-blue-500 hover:shadow-lg hover:shadow-blue-600/70"
            @click="incrementRight(1)">+ 1
        </button>
        <button
            class="button bg-gradient-to-br from-violet-500 to-fuchsia-500 hover:shadow-lg hover:shadow-fuchsia-700/70"
            @click="incrementRight(2)">+ 2
        </button>
        <button
            class="button bg-gradient-to-br from-red-500 to-amber-500 hover:shadow-lg hover:shadow-amber-700/70"
            @click="incrementRight(3)">+ 3
        </button>
      </div>

    </div>
    <div class="text-white h-8">

      <button
          class="text-slate-700 px-6 py-2"
          v-if="!resetQuestion && !winner"
          @click="resetQuestion = true"
      >Reset
      </button>

      <button
          class="px-6 py-2 bg-gradient-to-br from-emerald-700 to-green-700 rounded-sm hover:scale-105 duration-150"
          v-if="winner"
          @click="resetCounter"
      >New Game
      </button>

      <div
          class="flex flex-col justify-center items-center space-y-4"
          v-if="resetQuestion"
      >
        <p>Do you really want to reset?</p>
        <div class="space-x-4">
          <button
              class="px-6 py-2 bg-gradient-to-br from-pink-800 to-red-700 rounded-sm hover:scale-105 duration-150"
              @click="resetCounter"
          >Yes
          </button>
          <button
              class="px-6 py-2 bg-gradient-to-br from-emerald-700 to-green-800 rounded-sm hover:scale-105 duration-150"
              @click="resetQuestion = false"
          >No
          </button>
        </div>
      </div>
    </div>
  </main>
</template>
