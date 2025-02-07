<template>
    <div class="bg-gray-900 p-4">
      <h1 class="text-5xl text-center font-bold text-white">
        Rick and Morty Characters
      </h1>
  
      <!-- Botones de paginación -->
      <div v-if="page === 1" class="flex mt-6 mr-6 justify-end">
        <div>
          <button
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded m-4"
            @click="nextPage"
          >
            Next
          </button>
        </div>
      </div>
      <div v-else class="flex mt-6 mr-6 flex-row">
        <div class="basis-1/2">
          <button
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded m-4"
            @click="previousPage"
          >
            Previous
          </button>
        </div>
        <div class="basis-1/2 text-end">
          <button
            class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded m-4"
            @click="nextPage"
          >
            Next
          </button>
        </div>
      </div>
  
      <!-- Lista de personajes -->
      <div class="grid grid-cols-4">
        <div v-for="character in characters" :key="character.id" class="bg-slate-700 rounded-xl p-2 max-w-xs m-3">
          <img :src="character.image" />
          {{ character.name }}
        </div>
      </div>
    </div>
  </template>


<script setup>
import { onMounted, ref } from 'vue';


const characters = ref([]);
const page = ref(1);

// Función para cargar personajes
async function loadCharacters() {
  const response = await fetch(`https://rickandmortyapi.com/api/character?page=${page.value}`);
  const data = await response.json();
  
  characters.value = data.results;
  console.log(characters.value);
}

function nextPage() {
  page.value++; // Incrementa directamente
  console.log(page.value);
  loadCharacters();
}

function previousPage() {
  page.value--; // Decrementa directamente
  console.log(page.value);
  loadCharacters();
}

onMounted(() => {
  loadCharacters();
});
</script>