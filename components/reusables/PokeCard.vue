<script setup lang="ts">
defineProps({
  name: String,
  imageUrl: String,
  id: String,
})
</script>
<template>
  <div class="border-neutral-300 rounded-lg border flex flex-col h-32 items-center justify-center capitalize">
    {{ name }}
  </div>
</template>