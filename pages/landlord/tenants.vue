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
                      <td class="py-3 pr-4 font-semibold text-slate-900">
                        <button
                          type="button"
                          class="text-left text-[#00696b] hover:text-[#004d4f] hover:underline"
                          @click="openTenantDetails(tenant)"
                        >
                          {{ tenant.name }}
                        </button>
                      </td>
                      <td class="py-3 pr-4 text-slate-700">{{ tenant.unit }}</td>
                      <td class="py-3 pr-4 text-slate-700">{{ tenant.phone }}</td>
                      <td class="py-3 text-slate-700">{{ tenant.status }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
          </section>

          <div
            v-if="selectedTenant"
            class="fixed inset-0 z-50 flex items-center justify-center bg-slate-900/50 p-4"
            @click.self="closeTenantDetails"
          >
            <section class="w-full max-w-2xl rounded-2xl bg-white border border-slate-200 shadow-2xl p-5 sm:p-6">
              <header class="flex items-start justify-between gap-4">
                <div>
                  <h2 class="text-2xl font-bold text-slate-900">{{ selectedTenant.name }}</h2>
                  <p class="mt-1 text-sm text-slate-500">Tenant profile details</p>
                </div>
                <button
                  type="button"
                  class="h-9 w-9 rounded-lg border border-slate-200 text-slate-600 hover:text-slate-900 hover:bg-slate-100"
                  aria-label="Close details popup"
                  @click="closeTenantDetails"
                >
                  x
                </button>
              </header>

              <div class="mt-5 grid grid-cols-1 sm:grid-cols-2 gap-4 text-sm">
                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Unit</p>
                  <p class="mt-1 font-semibold text-slate-900">{{ selectedTenant.unit }}</p>
                </article>

                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Status</p>
                  <p class="mt-1 font-semibold" :class="selectedTenant.status === 'Current' ? 'text-emerald-700' : 'text-amber-700'">
                    {{ selectedTenant.status }}
                  </p>
                </article>

                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Phone</p>
                  <p class="mt-1 font-semibold text-slate-900">{{ selectedTenant.phone }}</p>
                </article>

                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Email</p>
                  <p class="mt-1 font-semibold text-slate-900 break-all">{{ selectedTenant.email }}</p>
                </article>

                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">National ID</p>
                  <p class="mt-1 font-semibold text-slate-900">{{ selectedTenant.nationalId }}</p>
                </article>

                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Monthly Rent</p>
                  <p class="mt-1 font-semibold text-slate-900">{{ selectedTenant.monthlyRent }}</p>
                </article>

                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Lease Start</p>
                  <p class="mt-1 font-semibold text-slate-900">{{ selectedTenant.leaseStart }}</p>
                </article>

                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Lease End</p>
                  <p class="mt-1 font-semibold text-slate-900">{{ selectedTenant.leaseEnd }}</p>
                </article>

                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4 sm:col-span-2">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Emergency Contact</p>
                  <p class="mt-1 font-semibold text-slate-900">{{ selectedTenant.emergencyContact }}</p>
                </article>
              </div>

              <footer class="mt-6 flex justify-end">
                <button
                  type="button"
                  class="inline-flex items-center justify-center rounded-lg bg-[#00696b] px-4 py-2 text-sm font-semibold text-white hover:bg-[#004d4f] transition-colors"
                  @click="closeTenantDetails"
                >
                  Close
                </button>
              </footer>
            </section>
          </div>
        </section>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import LandlordSidebar from '~/components/landlord/LandlordSidebar.vue'

type Tenant = {
  name: string
  unit: string
  phone: string
  status: 'Current' | 'Late'
  email: string
  nationalId: string
  monthlyRent: string
  leaseStart: string
  leaseEnd: string
  emergencyContact: string
}

const tenants: Tenant[] = [
  {
    name: 'A. Mugo',
    unit: '101',
    phone: '+254 700 000 101',
    status: 'Current',
    email: 'amugo@example.com',
    nationalId: '28745631',
    monthlyRent: 'Ksh 45,000',
    leaseStart: 'Jan 1, 2026',
    leaseEnd: 'Dec 31, 2026',
    emergencyContact: 'M. Mugo (+254 711 220 100)'
  },
  {
    name: 'N. Wanjiku',
    unit: '205',
    phone: '+254 700 000 205',
    status: 'Current',
    email: 'nwanjiku@example.com',
    nationalId: '30219874',
    monthlyRent: 'Ksh 38,500',
    leaseStart: 'Feb 1, 2026',
    leaseEnd: 'Jan 31, 2027',
    emergencyContact: 'P. Wanjiku (+254 722 880 205)'
  },
  {
    name: 'K. Otieno',
    unit: '301',
    phone: '+254 700 000 301',
    status: 'Late',
    email: 'kotieno@example.com',
    nationalId: '26501983',
    monthlyRent: 'Ksh 41,250',
    leaseStart: 'Mar 1, 2026',
    leaseEnd: 'Feb 28, 2027',
    emergencyContact: 'J. Akinyi (+254 733 990 301)'
  },
  {
    name: 'L. Kilonzo',
    unit: '412',
    phone: '+254 700 000 412',
    status: 'Current',
    email: 'lkilonzo@example.com',
    nationalId: '31800265',
    monthlyRent: 'Ksh 39,000',
    leaseStart: 'Apr 1, 2026',
    leaseEnd: 'Mar 31, 2027',
    emergencyContact: 'S. Kilonzo (+254 744 660 412)'
  }
]

const selectedTenant = ref<Tenant | null>(null)

function openTenantDetails(tenant: Tenant) {
  selectedTenant.value = tenant
}

function closeTenantDetails() {
  selectedTenant.value = null
}
</script>
