<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <LandlordSidebar />

    <div class="flex-1 flex flex-col">
      <main class="p-4 sm:p-6 flex-1">
        <section class="max-w-6xl mx-auto space-y-6">
          <header class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
            <div>
              <h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Financials</h1>
              <p class="mt-2 text-slate-500 text-sm sm:text-base">Review rent totals, collections, and recent payment activity.</p>
            </div>
            <NuxtLink
              to="/landlord/dashboard"
              class="inline-flex items-center justify-center rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100 transition-colors"
            >
              Back to Dashboard
            </NuxtLink>
          </header>

          <section class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-5">
            <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
              <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                <p class="text-xs uppercase tracking-wide text-slate-500">Expected Rent</p>
                <p class="mt-2 text-2xl font-semibold text-slate-900">Ksh 2.3M</p>
              </article>
              <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                <p class="text-xs uppercase tracking-wide text-slate-500">Collected</p>
                <p class="mt-2 text-2xl font-semibold text-emerald-700">Ksh 2.1M</p>
              </article>
              <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                <p class="text-xs uppercase tracking-wide text-slate-500">Outstanding</p>
                <p class="mt-2 text-2xl font-semibold text-rose-700">Ksh 0.2M</p>
              </article>
            </div>

            <div class="space-y-3">
              <h2 class="text-xl font-semibold text-slate-900">Recent Collections</h2>
              <div class="overflow-x-auto">
                <table class="min-w-full text-left text-sm">
                  <thead>
                    <tr class="border-b border-slate-100 text-slate-500">
                      <th class="py-2 pr-4 font-medium">Tenant</th>
                      <th class="py-2 pr-4 font-medium">Amount</th>
                      <th class="py-2 font-medium">Date</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="payment in payments" :key="payment.tenant" class="border-b border-slate-50">
                      <td class="py-3 pr-4 font-semibold text-slate-900">{{ payment.tenant }}</td>
                      <td class="py-3 pr-4 text-emerald-700 font-semibold">{{ payment.amount }}</td>
                      <td class="py-3 text-slate-700">{{ payment.date }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </section>
        </section>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import LandlordSidebar from '~/components/landlord/LandlordSidebar.vue'

const payments = [
  { tenant: 'A. Mugo', amount: 'Ksh 45,000', date: 'Apr 20, 2026' },
  { tenant: 'N. Wanjiku', amount: 'Ksh 38,500', date: 'Apr 18, 2026' },
  { tenant: 'K. Otieno', amount: 'Ksh 41,250', date: 'Apr 17, 2026' }
]
</script>
