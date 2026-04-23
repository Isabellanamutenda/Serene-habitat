<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <LandlordSidebar />

    <div class="flex-1 flex flex-col">
      <main class="p-4 sm:p-6 flex-1">
        <section class="max-w-6xl mx-auto space-y-6">
          <header class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
            <div>
              <h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Maintenance</h1>
              <p class="mt-2 text-slate-500 text-sm sm:text-base">Manage active maintenance work and ticket priority.</p>
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
                <p class="text-xs uppercase tracking-wide text-slate-500">Open Tickets</p>
                <p class="mt-2 text-2xl font-semibold text-slate-900">8</p>
              </article>
              <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                <p class="text-xs uppercase tracking-wide text-slate-500">High Priority</p>
                <p class="mt-2 text-2xl font-semibold text-rose-700">2</p>
              </article>
              <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                <p class="text-xs uppercase tracking-wide text-slate-500">Resolved This Week</p>
                <p class="mt-2 text-2xl font-semibold text-emerald-700">14</p>
              </article>
            </div>

            <div class="space-y-3">
              <h2 class="text-xl font-semibold text-slate-900">Maintenance Queue</h2>
              <ul class="space-y-3">
                <li v-for="ticket in tickets" :key="ticket.unit" class="rounded-xl border border-slate-100 p-4">
                  <div class="flex items-start justify-between gap-4">
                    <div>
                      <p class="font-semibold text-slate-900">{{ ticket.unit }} • {{ ticket.issue }}</p>
                      <p class="mt-1 text-xs text-slate-500">{{ ticket.age }}</p>
                    </div>
                    <span class="rounded-full px-2.5 py-1 text-xs font-semibold" :class="ticket.priority === 'High' ? 'bg-rose-100 text-rose-700' : ticket.priority === 'Medium' ? 'bg-amber-100 text-amber-700' : 'bg-emerald-100 text-emerald-700'">
                      {{ ticket.priority }}
                    </span>
                  </div>
                </li>
              </ul>
            </div>
          </section>
        </section>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import LandlordSidebar from '~/components/landlord/LandlordSidebar.vue'

const tickets = [
  { unit: '101', issue: 'Kitchen sink leak', age: '2 hrs old', priority: 'High' },
  { unit: '205', issue: 'Door lock battery low', age: '5 hrs old', priority: 'Medium' },
  { unit: '412', issue: 'Lighting flicker', age: '1 day old', priority: 'Low' }
]
</script>
