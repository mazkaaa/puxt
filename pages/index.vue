<script setup lang="ts">
import { ref } from 'vue';
import PokeCard from '~/components/reusables/PokeCard.vue';

type fetchResultType = {
  name: string
  url: string
}

const currentData = ref(0)
const pokemons = ref<fetchResultType[]>([])
const loading = ref(false)

const fetchData = async () => {
  currentData.value += 30
  loading.value = true
  try {
    const { data } = await useFetch("https://pokeapi.co/api/v2/pokemon?limit=" + currentData.value +"&offset=0");
    pokemons.value = [...pokemons.value, ...(data.value as any).results]
  } catch (error) {
    console.log(error)
  }
  loading.value = false
}

fetchData()
</script>

<template>
  <div class="my-8 mx-6 space-y-8">
    <h1 class="text-4xl font-semibold text-center">Pokemons</h1>
    <div class="grid grid-cols-12 gap-3">
      <div v-for="poke in pokemons" class="col-span-1">
        <PokeCard :name="poke.name" />
      </div>
    </div>
    <div class="flex justify-center items-center">
      <button :disabled="loading" @click="fetchData" class="btn btn-primary">{{ loading ? 'Loading...' : 'Load More' }}</button>
    </div>
  </div>
</template>