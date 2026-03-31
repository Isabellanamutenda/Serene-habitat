<template>
  <div class="bg-white rounded-[2.5rem] p-8 shadow-sm border border-slate-50 flex flex-col gap-6">
    <div class="flex items-start justify-between gap-4">
      <div>
        <h3 class="text-xl font-bold text-slate-800">Grace Period</h3>
        <p class="text-slate-400 text-sm">Payment extension window</p>
      </div>
      <span
        class="text-[10px] px-3 py-1 rounded-full font-bold uppercase tracking-tighter"
        :class="badgeClass"
      >
        {{ stateLabel }}
      </span>
    </div>

    <div class="text-center">
      <p class="text-4xl font-black" :class="valueClass">{{ daysRemaining }} Days</p>
      <p class="text-slate-500 text-sm mt-1">Remaining before penalty</p>
    </div>

    <p class="text-center text-xs font-semibold" :class="valueClass">
      {{ message }}
    </p>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue'

const props = defineProps<{
  daysRemaining: number
}>()

const isCritical = computed(() => props.daysRemaining <= 3)
const isSafe = computed(() => props.daysRemaining >= 5)

const stateLabel = computed(() => {
  if (isCritical.value) return 'Critical'
  if (isSafe.value) return 'Healthy'
  return 'Warning'
})

const valueClass = computed(() => {
  if (isCritical.value) return 'text-red-600'
  if (isSafe.value) return 'text-green-600'
  return 'text-amber-600'
})

const badgeClass = computed(() => {
  if (isCritical.value) return 'bg-red-100 text-red-700'
  if (isSafe.value) return 'bg-green-100 text-green-700'
  return 'bg-amber-100 text-amber-700'
})

const message = computed(() => {
  if (isCritical.value) return 'Urgent action needed.'
  if (isSafe.value) return 'Grace window is healthy.'
  return 'Grace window is getting tighter.'
})
</script>
