<script setup lang="ts">
import type { PokemonInterface } from "~/components/interfaces/pokemon";

const route = useRoute();
const pokemonNameParam = route.params.name;
const { data, pending } = await useFetch(
  "https://pokeapi.co/api/v2/pokemon/" + pokemonNameParam,
  {
    lazy: true,
  }
);
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
</template>
