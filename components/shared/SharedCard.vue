<template>
  <div class="bg-white p-4 rounded-lg shadow">
    <h3 class="text-lg font-semibold mb-2 text-slate-800">{{ title }}</h3>
    <slot />
  </div>
</template>

<script setup lang="ts">
defineProps({
  title: String
})
</script>
