<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <!-- Sidebar -->
    <Sidebar />

    <!-- Main content -->
    <div class="flex-1 flex flex-col">
      <!-- Page content -->
      <main class="p-4 sm:p-6 flex-1">
  <div class="max-w-7xl mx-auto space-y-12">
    <header class="flex flex-col xl:flex-row xl:items-start xl:justify-between gap-6">
      <div>
        <h1 class="text-4xl sm:text-5xl xl:text-6xl font-bold text-slate-900 tracking-tight">
          {{ greeting }}, <br />
          <span class="text-[#00696b]">Ashley.</span>
        </h1>
        <p class="mt-4 text-slate-400 max-w-md text-base sm:text-lg leading-relaxed">
          Your habitat is optimized for comfort. Everything is in order for your peaceful stay today.
        </p>
      </div>

      <div class="flex flex-col sm:flex-row gap-4 w-full xl:w-auto">
        <div class="w-full sm:w-auto bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6">
          <div class="text-xs font-semibold uppercase tracking-wide text-slate-500">Rent per Day</div>
          <div class="mt-2 text-slate-900 font-bold text-3xl leading-none">Ksh 4,500</div>
          <div class="mt-1 text-sm text-slate-500">Fully inclusive</div>
          <NuxtLink
            to="/tenant/details"
            class="mt-4 w-full inline-flex items-center justify-center bg-[#00696b] text-white py-2 rounded-lg font-semibold text-sm hover:bg-[#004d4f] transition-all"
          >
            View Details
          </NuxtLink>
        </div>
      </div>
    </header>

    <div class="space-y-8">
      <div class="grid grid-cols-1 xl:grid-cols-12 gap-6 items-start">
        <div class="xl:col-span-7">
          <Rewards :points="850" :goal="1000" streak="12 Months" />
        </div>
        <div class="xl:col-span-5">
          <GracePeriodCard :days-remaining="graceDaysRemaining" />
        </div>
      </div>
      <div id="transactions">
        <PaymentHistory :transactions="transactions" />
      </div>
    </div>
  </div>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref, computed } from 'vue'
import Sidebar from '~/components/shared/SharedSidebar.vue'
import GracePeriodCard from '~/components/landlord/GracePeriodCard.vue'
import Rewards from '~/components/tenant/Rewards.vue'
import PaymentHistory from '~/components/tenant/PaymentHistory.vue'

const graceDaysRemaining = 5

const transactions = [
  { date: 'Oct 01, 2023', ref: 'MP7XW92K01', amount: 'Ksh 4,500.00', status: 'Completed' },
  { date: 'Sep 30, 2023', ref: 'MP2RQ11L45', amount: 'Ksh 4,500.00', status: 'Completed' },
  { date: 'Sep 29, 2023', ref: 'MP8N55M92', amount: 'Ksh 4,500.00', status: 'Completed' }
]

const currentHour = ref(new Date().getHours())
let clockTimer: ReturnType<typeof setInterval> | undefined

onMounted(() => {
  clockTimer = setInterval(() => {
    currentHour.value = new Date().getHours()
  }, 60000)
})

onBeforeUnmount(() => {
  if (clockTimer) clearInterval(clockTimer)
})

const greeting = computed(() => {
  if (currentHour.value < 12) return 'Good morning'
  if (currentHour.value < 18) return 'Good afternoon'
  return 'Good evening'
})
</script>