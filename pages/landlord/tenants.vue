<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <LandlordSidebar />

    <div class="flex-1 flex flex-col">
      <main class="p-4 sm:p-6 flex-1">
        <section class="max-w-6xl mx-auto space-y-6">
          <header class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
            <div>
              <h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Tenants</h1>
              <p class="mt-2 text-slate-500 text-sm sm:text-base">Review tenant records, upcoming payments, and contact status.</p>
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
                <p class="text-xs uppercase tracking-wide text-slate-500">Total Tenants</p>
                <p class="mt-2 text-2xl font-semibold text-slate-900">41</p>
              </article>
              <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                <p class="text-xs uppercase tracking-wide text-slate-500">On Time</p>
                <p class="mt-2 text-2xl font-semibold text-emerald-700">36</p>
              </article>
              <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                <p class="text-xs uppercase tracking-wide text-slate-500">Requires Follow Up</p>
                <p class="mt-2 text-2xl font-semibold text-amber-700">5</p>
              </article>
            </div>

            <div class="space-y-3">
              <h2 class="text-xl font-semibold text-slate-900">Tenant List</h2>
              <div class="overflow-x-auto">
                <table class="min-w-full text-left text-sm">
                  <thead>
                    <tr class="border-b border-slate-100 text-slate-500">
                      <th class="py-2 pr-4 font-medium">Tenant</th>
                      <th class="py-2 pr-4 font-medium">Unit</th>
                      <th class="py-2 pr-4 font-medium">Phone</th>
                      <th class="py-2 font-medium">Status</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr v-for="tenant in tenants" :key="tenant.name" class="border-b border-slate-50">
                      <td class="py-3 pr-4 font-semibold text-slate-900">{{ tenant.name }}</td>
                      <td class="py-3 pr-4 text-slate-700">{{ tenant.unit }}</td>
                      <td class="py-3 pr-4 text-slate-700">{{ tenant.phone }}</td>
                      <td class="py-3 text-slate-700">{{ tenant.status }}</td>
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

const tenants = [
  { name: 'A. Mugo', unit: '101', phone: '+254 700 000 101', status: 'Current' },
  { name: 'N. Wanjiku', unit: '205', phone: '+254 700 000 205', status: 'Current' },
  { name: 'K. Otieno', unit: '301', phone: '+254 700 000 301', status: 'Late' },
  { name: 'L. Kilonzo', unit: '412', phone: '+254 700 000 412', status: 'Current' }
]
</script>
