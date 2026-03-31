<template>
  <div class="bg-[#c2f0f2] rounded-[2.5rem] p-8 flex flex-col lg:flex-row gap-8 items-center border border-cyan-100/50">
    <div class="flex-1 space-y-4">
      <h3 class="text-2xl font-bold text-slate-800">Habitat Loyalty</h3>
      <p class="text-slate-500 text-sm leading-relaxed max-w-xs">
        You're <span class="font-bold text-[#00696b]">{{ progress }}%</span> of the way to a complimentary spa session at the building's wellness center. Keep it up!
      </p>
      
      <div class="pt-4">
        <div class="flex items-baseline gap-1 mb-2">
          <span class="text-3xl font-bold text-slate-800">{{ points }}</span>
          <span class="text-slate-400 font-medium">/ {{ goal }} pts</span>
        </div>
        <div class="w-full bg-white/50 rounded-full h-3 overflow-hidden">
          <div 
            class="bg-[#00696b] h-full rounded-full transition-all duration-1000 ease-out" 
            :style="{ width: progress + '%' }"
          ></div>
        </div>
      </div>
    </div>

    <div class="bg-white/60 backdrop-blur-sm rounded-[2rem] p-6 text-center border border-white/40 shadow-sm w-full lg:w-48">
      <div class="text-[#00696b] text-2xl mb-2">✨</div>
      <p class="text-[10px] uppercase tracking-widest font-bold text-slate-400 mb-1">Current Streak</p>
      <p class="text-xl font-black text-slate-800">{{ streak }}</p>
      <p class="text-[9px] uppercase font-bold text-[#00696b] mt-2 tracking-tighter">Perfect Payment Record</p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  points: number
  goal: number
  streak: string
}>()

const progress = computed(() => Math.round((props.points / props.goal) * 100))
</script>