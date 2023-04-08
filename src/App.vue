<script setup>
  import { ref, computed } from 'vue'  
  import { useDark, useToggle } from '@vueuse/core'
  
  const isDark = useDark();
  const toggleDark = useToggle(isDark);

  const player = ref('X');
  const board = ref([
    ['', '', ''],
    ['', '', ''],
    ['', '', ''],
  ]);

  const calculateWinner = (board) => {
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
      if (board[a] && board[a] === board[b] && board[a] === board[c]) {
        return board[a];
      }
    }
    return null;
  }

  const winner = computed(() => calculateWinner(board.value.flat()))

  function makeMove(x, y){
    if(winner.value) return

    if(board.value[x][y]) return

    board.value[x][y] = player.value

    player.value = player.value === 'X' ? 'O' : 'X'
  }

  const resetGame = () =>{
     board.value = [
      ['', '', ''],
      ['', '', ''],
      ['', '', ''],
    ]
    player.value = 'X';
  }
</script>


<template>

  <main class="pt-8 text-center dark:bg-gray-800 min-h-screen dark:text-white light:text-black">
    <h1 class="mb-5 text-3xl font-bold uppercase">Tic Tac Toe</h1>
    <h2 class="mb-5 text-xl font-bold">X - O</h2>
    <h3 class="text-xl mb-5">{{ player }} turn</h3>

    <div class="flex flex-col items-center mb-8">
      <div v-for="(row, x) in board" :key="x" class="flex">

        <div v-for="(cell, y) in row" :key="y" @click="makeMove(x, y)" 
            :class="`border border-gray-800 w-20 h-20 flex items-center justify-center
            material-icons-outlined text-4xl cursor-pointer dark:border-white ${ cell === 'X' ? 'text-blue-500' : 'text-red-500' }`">
            {{ cell === 'X' ? 'X' : cell === 'O' ? 'O' : '' }}
        </div>
      </div>
    </div>

    <button @click="resetGame" class="mb-5 px-4 py-2 bg-gray-800 rounded text-white dark:text-gray-800 dark:bg-white">Reset</button>





    <div class="mb-5 flex items-center justify-center">
      <button @click="toggleDark()" class="px-4 py-2 bg-gray-800  rounded text-white dark:text-gray-800 dark:bg-white">{{ isDark ? 'Light' : 'Dark' }} Mode</button>
    </div>

    <h3 v-if="winner" class="text-2xl font-bold mb-8">Player '{{ winner }}' wins!</h3>

  </main>  

</template>


<style>

</style>