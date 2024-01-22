<script setup lang="ts">
import { ref } from 'vue';

const { data, error } = await useFetch("https://pokeapi.co/api/v2/pokemon");

const results = ref((data.value as any).results);
</script>

<template>
  <div class="my-4 mx-6 space-y-8">
    <h1 class="text-4xl font-semibold text-center">Pokemons</h1>
    <div class="grid grid-cols-12 gap-4">
      <div v-for="poke in results">
        <ReusablesPokeCard class="col-span-3" />
      </div>
    </div>
  </div>
</template>