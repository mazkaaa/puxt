<script setup lang="ts">
import type { PokemonInterface } from "../interfaces/pokemon";
const props = defineProps({
  pokeUrl: String,
})

const { data } = await useFetch(props.pokeUrl || "", {
  lazy: true,
});

const poke: PokemonInterface = data as any

</script>
<template>
  <div class="border-neutral-300 rounded-lg border flex flex-col h-40 w-40 items-center justify-center capitalize group">
    <img :src="poke.sprites.front_default" :alt="poke.name" class="w-3/4 group-hover:hidden" />
    <div class="bg-neutral-300 w-full h-full group-hover:flex hidden rounded-lg text-base-300 flex-col justify-center items-center">
      <h3 class="text-lg font-semibold">{{ poke.name }}</h3>
    </div>
  </div>
</template>