<script setup lang="ts">
import { ref } from "vue";
import PokeCard from "~/components/reusables/PokeCard.vue";

type fetchResultType = {
  name: string;
  url: string;
};

const currentData = ref(24);
const { data, pending, status } = await useFetch(
  "https://pokeapi.co/api/v2/pokemon",
  {
    query: {
      limit: currentData,
      offset: 0,
    },
    lazy: true,
    watch: [currentData],
  }
);
</script>

<template>
  <div class="space-y-8">
    <h1 class="text-4xl font-semibold text-center">Pokemons</h1>
    <div class="flex flex-row flex-wrap gap-3 justify-center">
      <div
        v-if="status === 'success'"
        v-for="poke in ((data as any).results as fetchResultType[])"
      >
        <NuxtLink :to="`/pokemon/${poke.name}`">
          <PokeCard :poke-url="poke.url" />
        </NuxtLink>
      </div>
    </div>
    <div class="flex justify-center items-center">
      <button
        @click="currentData += 24"
        :disabled="pending"
        class="btn btn-primary"
      >
        {{ pending ? "Loading" : "Load more" }}
        <span v-if="pending" class="loading loading-ring"></span>
      </button>
    </div>
  </div>
</template>
