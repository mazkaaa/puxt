<script setup lang="ts">
import { ref } from "vue";
import PokeCard from "~/components/reusables/PokeCard.vue";

type fetchResultType = {
  name: string;
  url: string;
};

const currentData = ref(30);
const pokemons = ref<fetchResultType[]>([]);
const loading = ref(false);
const { data: { value }, pending } = await useFetch(
  "https://pokeapi.co/api/v2/pokemon", {
    query: {
      limit: currentData.value,
      offset: 0,
    },
    lazy: true,
    watch: [currentData],
  }
);
console.log(value)
</script>

<template>
  <div class="my-8 mx-6 space-y-8">
    <h1 class="text-4xl font-semibold text-center">Pokemons</h1>
    <div class="grid grid-cols-12 gap-3">
      <!-- <div v-for="poke in results" class="col-span-1"> -->
        <!-- <PokeCard :name="poke.name" /> -->
      </div>
    </div>
    <div class="flex justify-center items-center">
      <button @click="currentData+=30" :disabled="pending" class="btn btn-primary">
        {{ pending ? "Loading..." : "Load More" }}
      </button>
    </div>
  </div>
</template>
