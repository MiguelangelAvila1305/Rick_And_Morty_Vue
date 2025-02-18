<template>


  <button @click="()=>router.push('/')" class="p-3 ml-3 mt-3 text-xl font-semibold text-white bg-black rounded-2xl">Volver</button>
<div class="flex justify-center items-center pt-10 lg:h-[500px] ">

  <div class="grid grid-cols-1  mx-auto gap-5 lg:grid-cols-2 lg:w-6xl ">
    <div>
      <img class="w-96 rounded-3xl transition duration-400 hover:scale-105 " :src="character?.image" :alt="character?.name" />
    </div>

    <div class="shadow-2xl p-4 rounded-2xl lg:p-8 lg:text-xl">
      <h1 class="text-4xl font-bold text-center">{{ character?.name }}</h1>
      <div class="text-center space-y-2 mt-8 lg:space-y-4 lg:text-start">
        <p><span class="font-semibold">Estado:</span> {{ character?.status }}</p>
        <p><span class="font-semibold">Especie:</span> {{ character?.species }}</p>
        <p><span class="font-semibold">Género:</span> {{ character?.gender }}</p>
        <p><span class="font-semibold">Origen:</span> {{ character?.origin?.name }}</p>
        <p><span class="font-semibold">Ubicación:</span> {{ character?.location?.name }}</p>
      </div>
    </div>
  </div>
</div>
</template>

<script setup lang="ts">
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";
import { useRouter } from "vue-router";

const router = useRouter();
const route = useRoute();
const id = route.params.id;
const character = ref();

const loadCharacter = async () => {
  const response = await fetch(
    `https://rickandmortyapi.com/api/character/${id}`
  );
  const data = await response.json();
  console.log(data);
  character.value = data;
  console.log(character.value);
};

onMounted(() => {
  loadCharacter();
});
</script>
