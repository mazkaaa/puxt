<script setup lang="ts">
import type { PokemonInterface } from "../interfaces/pokemon";
const props = defineProps({
  id: {
    type: Number,
    required: false,
  },
  name: {
    type: String,
    required: false,
  },
  image_url: {
    type: String,
    required: false,
  },
  skeleton: {
    type: Boolean,
    default: false,
  },
  owned: {
    type: Boolean,
    default: false,
  }
});
</script>
<template>
  <div
    class="rounded-lg bg-base-300 flex flex-col h-40 w-40 items-center justify-center capitalize group"
    :class="(props.skeleton ? 'skeleton' : '') + (props.owned ? ' border-green-900 border' : '')"
  >
    <img
      :src="props.image_url"
      :alt="props.name"
      class="w-3/4 group-hover:hidden"
    />
    <div
      class="bg-neutral-300 w-full h-full group-hover:flex hidden rounded-lg text-base-300 flex-col justify-center items-center"
    >
      <h3 class="text-lg font-semibold">{{ props.name }}</h3>
    </div>
  </div>
</template>
