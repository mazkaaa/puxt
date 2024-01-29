<script setup lang="ts">
import { ref } from "vue";
import type { PokemonInterface } from "~/components/interfaces/pokemon";
import PokeCard from "~/components/reusables/PokeCard.vue";

type fetchResultType = {
  name: string;
  url: string;
};

const currentData = ref(24);
const api_url = "https://pokeapi.co/api/v2/pokemon";

const {
  data: pokemons,
  pending,
} = await useLazyAsyncData(
  "pokemons",
  async () => {
    const pokeIndex = await $fetch(api_url, {
      query: {
        limit: currentData.value,
        offset: 0,
      },
    });
    const pokeIndexData = (pokeIndex as any).results as fetchResultType[];
    const pokeData = await Promise.all(
      pokeIndexData.map(async (poke) => {
        const result = await $fetch(poke.url);
        return result;
      })
    );
    return pokeData;
  },
  {
    watch: [currentData],
    server: true,
  }
);
</script>

<template>
  <div class="space-y-8">
    <h1 class="text-4xl font-semibold text-center">Pokemons</h1>
    <div class="flex flex-row flex-wrap gap-3 justify-center">
      <div v-if="!pending" v-for="poke in (pokemons as PokemonInterface[])">
        <NuxtLink :to="`/pokemon/${poke.name}`">
          <PokeCard
            :image_url="poke.sprites.front_default"
            :name="poke.name"
            :id="poke.id"
          />
        </NuxtLink>
      </div>

      <div v-else v-for="index in currentData">
        <PokeCard :key="index" :skeleton="true" />
      </div>
    </div>
    <div class="flex justify-center items-center">
      <button
        :disabled="pending"
        @click="currentData += 24"
        class="btn btn-primary"
      >
        {{ pending ? "Loading" : "Load more" }}
        <span v-if="pending" class="loading loading-ring"></span>
      </button>
    </div>
  </div>
</template>
