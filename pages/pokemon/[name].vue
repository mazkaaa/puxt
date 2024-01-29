<script setup lang="ts">
import { type Ability, type PokemonInterface } from "~/components/interfaces/pokemon";

const route = useRoute();
const pokemonNameParam = route.params.name;
const { data: pokemon, pending } = await useLazyFetch<PokemonInterface>(
  "https://pokeapi.co/api/v2/pokemon/" + pokemonNameParam,
);

const {data: abilities} = await useLazyAsyncData("abilities", async () => {
  const abilitiesUrl = pokemon?.value?.abilities.map((ability: Ability) => {
    return ability.ability.url;
  });
  const abilitiesData = await Promise.all(
    (abilitiesUrl as any[])?.map(async (url: string) => {
      const result = await $fetch(url);
      return result;
    })
  );
  return abilitiesData;
})
</script>

<template>
  <header class="grid grid-cols-3 items-center">
    <div class="col-span-1">
      <NuxtLink to="/" class="btn btn-error">Back to homepage</NuxtLink>
    </div>
    <div class="col-span-1">
      <h1 class="text-center text-4xl capitalize font-semibold">
        {{ pokemonNameParam }}
      </h1>
    </div>
  </header>

  <main class="mt-12 space-y-6">
    <div class="flex justify-center">
      <img :src="pokemon?.sprites.front_default" class="" />
      <img :src="pokemon?.sprites.back_default" class="" />
      <img :src="pokemon?.sprites.front_shiny" class="" />
    </div>

    <div class="flex justify-center flex-col items-center space-y-2">
      <h4 class="text-xl font-medium">Abilities</h4>
      <div class="flex flex-row space-x-2">
        <p class="border-dotted border-b" v-for="ability in pokemon?.abilities">{{ ability.ability.name }}</p>
      </div>
    </div>

    <div class="flex justify-center flex-col items-center space-y-2">
      <h4 class="text-xl font-medium">Moves</h4>
      <div class="flex flex-row space-x-2 flex-wrap justify-center">
        <p class="border-dotted border-b" v-for="move in pokemon?.moves">{{ move.move.name }}</p>
      </div>
    </div>

    <div class="flex justify-center flex-col items-center space-y-2">
      <h4 class="text-xl font-medium">Forms</h4>
      <div class="flex flex-row space-x-2 flex-wrap justify-center">
        <p class="border-dotted border-b" v-for="form in pokemon?.forms">{{ form.name }}</p>
      </div>
    </div>

    <div class="flex justify-center">
      <button class="btn btn-primary">Catch pokemon</button>
    </div>
  </main>
</template>
