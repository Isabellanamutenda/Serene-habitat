<template>
  <div class="bg-white rounded-[2.5rem] p-8 shadow-sm border border-slate-50 flex flex-col items-center">
    <div class="w-full flex justify-between items-start mb-8">
      <div>
        <h3 class="text-xl font-bold text-slate-800">Entry Portal</h3>
        <p class="text-slate-400 text-sm">Unit {{ unit }} - Main Door</p>
      </div>
      <span class="bg-[#c2f5d3] text-[#1b4332] text-[10px] px-3 py-1 rounded-full font-bold uppercase tracking-tighter flex items-center gap-1">
        <span class="w-1.5 h-1.5 bg-[#1b4332] rounded-full animate-pulse"></span>
        MQTT LIVE
      </span>
    </div>

    <div class="relative flex items-center justify-center my-4">
      <svg class="w-48 h-48 transform -rotate-90">
        <circle cx="96" cy="96" r="80" stroke="currentColor" stroke-width="8" fill="transparent" class="text-slate-100" />
        <circle cx="96" cy="96" r="80" stroke="currentColor" stroke-width="8" fill="transparent" stroke-dasharray="502" stroke-dashoffset="150" class="text-[#00696b] transition-all duration-1000" />
      </svg>
      <div class="absolute inset-0 flex items-center justify-center">
        <span class="text-4xl text-[#00696b]">🔒</span>
      </div>
    </div>

    <div class="text-center mt-6">
      <p class="text-2xl font-bold text-slate-800">Status: {{ status }}</p>
      <p class="text-slate-400 text-xs mt-1">Last activity: {{ lastActivity }}</p>
    </div>

    <button @click="toggleLock" :class="isLocked ? 'bg-red-600 hover:bg-red-700' : 'bg-[#00696b] hover:bg-[#004d4f]'" class="w-full mt-8 text-white py-4 rounded-2xl font-bold flex items-center justify-center gap-2 transition-all disabled:opacity-50" :disabled="isLoading">
      <span>{{ isLocked ? '🔒' : '🔓' }}</span> {{ isLocked ? 'Lock Door' : 'Unlock Door' }}
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

defineProps<{
  unit: string | number
  status: string
  lastActivity: string
}>()

const isLocked = ref(true)
const isLoading = ref(false)

const toggleLock = async () => {
  isLoading.value = true
  try {
    // Simulate API call to lock/unlock door
    await new Promise(resolve => setTimeout(resolve, 500))
    isLocked.value = !isLocked.value
  } catch (error) {
    console.error('Failed to toggle lock:', error)
  } finally {
    isLoading.value = false
  }
}
</script>