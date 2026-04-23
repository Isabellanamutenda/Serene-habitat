<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <LandlordSidebar />

    <div class="flex-1 flex flex-col">
      <main class="p-4 sm:p-6 flex-1">
        <section class="max-w-6xl mx-auto space-y-6">
          <header class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
            <div>
              <h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Property Breakdown</h1>
              <p class="mt-2 text-slate-500 text-sm sm:text-base">View each property’s unit count and monthly rent totals.</p>
            </div>
            <NuxtLink
              to="/landlord/dashboard"
              class="inline-flex items-center justify-center rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100 transition-colors"
            >
              Back to Dashboard
            </NuxtLink>
          </header>

          <section class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-5">
            <div class="overflow-x-auto">
              <table class="min-w-full text-left text-sm">
                <thead>
                  <tr class="border-b border-slate-100 text-slate-500">
                    <th class="py-2 pr-4 font-medium">Property</th>
                    <th class="py-2 pr-4 font-medium">Location</th>
                    <th class="py-2 pr-4 font-medium">Units</th>
                    <th class="py-2 font-medium">Monthly Rent Total</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="property in properties" :key="property.name" class="border-b border-slate-50">
                    <td class="py-3 pr-4 font-semibold text-slate-900">{{ property.name }}</td>
                    <td class="py-3 pr-4 text-slate-700">{{ property.location }}</td>
                    <td class="py-3 pr-4 text-slate-700">{{ property.units }}</td>
                    <td class="py-3 font-semibold text-[#00696b]">{{ property.rent }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>
        </section>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import LandlordSidebar from '~/components/landlord/LandlordSidebar.vue'

const properties = [
  { name: 'Riverside Suites', location: 'Westlands', units: 18, rent: 'Ksh 1.2M' },
  { name: 'Downtown Plaza', location: 'CBD', units: 22, rent: 'Ksh 1.5M' },
  { name: 'Green Valley', location: 'Kileleshwa', units: 14, rent: 'Ksh 980K' }
]
</script>
