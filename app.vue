<script setup lang="ts">
useHead({
  title: "PUXT | Pokedex App",
  meta: [
    {
      name: "description",
      content: "A Pokedex App built with NuxtJS and TailwindCSS",
    },
  ],
})
</script>
<template>
  <NuxtLoadingIndicator />
  <NuxtLayout>
    <main class="my-8 mx-6">
      <NuxtPage />
    </main>
  </NuxtLayout>
</template>
