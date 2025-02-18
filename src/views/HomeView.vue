<template>
  <main>
    <div class="flex justify-center items-center pt-8">
      <h1 class="text-5xl font-bold">Rick y Morty</h1>
    </div>

    <!-- Input para buscar por nombre -->
   <div class="flex justify-center mt-8">
    <div >
      <input
        v-model="searchName"
        type="text"
        placeholder="Buscar por nombre..."
        class="p-2 border-2 border-gray-300 rounded-lg"
      />
    </div>

    <div class="space-x-4">
      <button v-for="status in statusFilters" :key="status" 
      :class="['bg-black text-white rounded-full p-2', selectedStatus === status? 'bg-blue-500' : 'bg-black'  ]"
      @click="filterByStatus(status)"
      > 
        {{ status }}
      </button>
    </div>
    
   </div>

    <!-- Paginación -->
    <div class="flex justify-center space-x-4 mt-8">
      <button class="border-2 bg-black text-white rounded-full w-10 h-10" @click="decrementPage">-</button>
      <div class="flex items-center">{{ page }}</div>
      <button class="border-2 bg-black text-white rounded-full w-10" @click="incrementPage">+</button>
    </div>

    <!-- Lista de personajes -->
    <div class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 gap-7 mt-24 mx-10">
      <div
        v-for="character in filteredCharacters"
        :key="character.id"
        class="rounded-2xl overflow-hidden shadow-2xl transition duration-300 hover:scale-105"
      >
        <div @click="seeCharacterDetails(character.id)">
          <img :src="character.image" :alt="character.name" />
          <div class="m-4 text-center">{{ character.name }}</div>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
import { onMounted, ref, computed } from 'vue'
import { useRouter } from 'vue-router'

const router = useRouter()
const characters = ref([])
const page = ref(1)
const searchName = ref('') // Para el input de búsqueda
const selectedStatus = ref('Todos') // Estado inicial: "Todos"

// Opciones de filtro por estado
const statusFilters = ['Todos', 'Alive', 'Dead', 'unknown']

// Cargar personajes
const loadCharacters = async () => {
  const response = await fetch(`https://rickandmortyapi.com/api/character?page=${page.value}`)
  const data = await response.json()
  characters.value = data.results
}

// Filtrar personajes por nombre y estado
const filteredCharacters = computed(() => {
  return characters.value.filter((character) => {
    const matchesName = character.name.toLowerCase().includes(searchName.value.toLowerCase())
    const matchesStatus = selectedStatus.value === 'Todos' || character.status === selectedStatus.value
    return matchesName && matchesStatus
  })
})

// Cambiar el estado seleccionado
const filterByStatus = (status: string) => {
  selectedStatus.value = status
  console.log(selectedStatus.value)
}

// Paginación
const incrementPage = () => {
  page.value++
  loadCharacters()
}

const decrementPage = () => {
  if (page.value > 1) {
    page.value--
    loadCharacters()
  }
}

// Ver detalles del personaje
const seeCharacterDetails = (character_id: number) => {
  router.push(`/details/${character_id}`)
}

// Cargar personajes al montar el componente
onMounted(() => {
  loadCharacters()
})
</script>

<style>
/* Estilos adicionales si los necesitas */
</style>