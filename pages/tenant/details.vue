<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <Sidebar />

    <div class="flex-1 flex flex-col">
      <main class="p-4 sm:p-6 flex-1">
        <section class="max-w-6xl mx-auto space-y-6">
          <header class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
            <div>
              <h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Rent Details</h1>
              <p class="mt-2 text-slate-500 text-sm sm:text-base">
                Overview of your rent, due dates, grace window, and payment methods.
              </p>
            </div>
            <NuxtLink
              to="/tenant/dashboard"
              class="inline-flex items-center justify-center rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100 transition-colors"
            >
              Back to Dashboard
            </NuxtLink>
          </header>

          <div class="grid grid-cols-1 lg:grid-cols-12 gap-4">
            <article class="card lg:col-span-4">
              <p class="text-sm text-slate-500">Rent Per Month</p>
              <p class="text-3xl font-semibold text-slate-900 mt-2">Ksh 4,500</p>
              <p class="text-xs text-slate-500 mt-2">Includes utilities and maintenance</p>
            </article>

            <article class="card lg:col-span-4">
              <p class="text-sm text-slate-500">Next Due Date</p>
              <p class="text-3xl font-semibold text-slate-900 mt-2">Apr 05, 2026</p>
              <p class="text-xs text-slate-500 mt-2">Auto reminders are enabled</p>
            </article>

            <article class="card lg:col-span-4">
              <p class="text-sm text-slate-500">Grace Period</p>
              <p class="text-3xl font-semibold text-green-600 mt-2">3 Days</p>
              <p class="text-xs text-slate-500 mt-2">No penalties during this period</p>
            </article>
          </div>

          <section class="card space-y-3">
            <h2 class="text-xl font-semibold text-slate-900">What You Can Do Here</h2>
            <ul class="space-y-2 text-sm text-slate-600">
              <li>Review your rent breakdown and service coverage.</li>
              <li>Check due dates and remaining grace period days.</li>
              <li>Confirm payment references and recent status updates.</li>
              <li>Use support request if you need billing assistance.</li>
            </ul>
          </section>
        </section>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import Sidebar from '~/components/shared/SharedSidebar.vue'
</script>
