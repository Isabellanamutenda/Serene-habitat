<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <Sidebar />

    <div class="flex-1 flex flex-col">
      <main class="p-4 sm:p-6 flex-1">
        <section class="max-w-6xl mx-auto space-y-6">
          <header class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
            <div>
              <h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Tenant Details</h1>
              <p class="mt-2 text-slate-500 text-sm sm:text-base">
                View your contact details, lease information, and financial history in one place.
              </p>
            </div>
            <NuxtLink
              to="/tenant/dashboard"
              class="inline-flex items-center justify-center rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100 transition-colors"
            >
              Back to Dashboard
            </NuxtLink>
          </header>

          <section class="card">
            <div class="flex flex-col gap-5 lg:flex-row lg:items-center lg:justify-between">
              <div>
                <p class="text-sm font-semibold uppercase tracking-wide text-[#00696b]">Apartment Tenant</p>
                <h2 class="mt-2 text-2xl font-bold text-slate-900">Ashley Mutenda</h2>
                <p class="mt-1 text-sm text-slate-500">Apartment A-14 · Unit 4B</p>
              </div>

              <div class="flex flex-col gap-3 sm:flex-row sm:flex-wrap">
                <div>
                  <p class="text-xs font-semibold uppercase tracking-wide text-slate-500">Email</p>
                  <p class="mt-1 text-sm font-medium text-slate-900">tenant@example.com</p>
                </div>
                <div>
                  <p class="text-xs font-semibold uppercase tracking-wide text-slate-500">Phone No</p>
                  <p class="mt-1 text-sm font-medium text-slate-900">+254 712 345 678</p>
                </div>
                <div>
                  <p class="text-xs font-semibold uppercase tracking-wide text-slate-500">Unit</p>
                  <p class="mt-1 text-sm font-medium text-slate-900">Unit 4B</p>
                </div>
                <div>
                  <p class="text-xs font-semibold uppercase tracking-wide text-slate-500">Apartment</p>
                  <p class="mt-1 text-sm font-medium text-slate-900">A-14</p>
                </div>
              </div>

              <div class="flex flex-wrap gap-2">
                <button
                  type="button"
                  class="inline-flex items-center justify-center rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100 transition-colors"
                  @click="openTenantPopup"
                >
                  Edit Profile
                </button>
                <button
                  type="button"
                  class="inline-flex items-center justify-center rounded-lg bg-slate-900 px-4 py-2 text-sm font-semibold text-white hover:opacity-95 transition-opacity"
                >
                  Message
                </button>
              </div>
            </div>
          </section>

          <div class="grid grid-cols-1 xl:grid-cols-5 gap-6 items-start">
            <section class="card xl:col-span-2 space-y-5">
              <div>
                <h2 class="text-xl font-semibold text-slate-900">Lease Information</h2>
                <p class="mt-1 text-sm text-slate-500">Your current lease summary and payment terms.</p>
              </div>

              <div class="grid grid-cols-1 sm:grid-cols-2 gap-4 text-sm">
                <div class="rounded-xl bg-[#f4f8f7] p-4">
                  <p class="text-xs font-semibold uppercase tracking-wide text-slate-500">Lease Start</p>
                  <p class="mt-2 text-base font-semibold text-slate-900">Jan 01, 2026</p>
                </div>
                <div class="rounded-xl bg-[#f4f8f7] p-4">
                  <p class="text-xs font-semibold uppercase tracking-wide text-slate-500">Lease End</p>
                  <p class="mt-2 text-base font-semibold text-slate-900">Dec 31, 2026</p>
                </div>
                <div class="rounded-xl bg-[#f4f8f7] p-4">
                  <p class="text-xs font-semibold uppercase tracking-wide text-slate-500">Monthly Rent</p>
                  <p class="mt-2 text-base font-semibold text-slate-900">Ksh 4,500</p>
                </div>
                <div class="rounded-xl bg-[#f4f8f7] p-4">
                  <p class="text-xs font-semibold uppercase tracking-wide text-slate-500">Grace Period</p>
                  <p class="mt-2 text-base font-semibold text-emerald-600">3 Days</p>
                </div>
              </div>

              <div class="rounded-xl border border-slate-200 bg-white p-4">
                <p class="text-sm font-semibold text-slate-900">Lease Notes</p>
                <p class="mt-2 text-sm leading-6 text-slate-600">
                  Utilities are included in the monthly rent. Maintenance requests should be sent through the message button.
                </p>
              </div>
            </section>

            <section class="card xl:col-span-3 space-y-5">
              <div>
                <h2 class="text-xl font-semibold text-slate-900">Financial History</h2>
                <p class="mt-1 text-sm text-slate-500">Recent rent payments and transaction status.</p>
              </div>

              <div class="overflow-x-auto rounded-xl border border-slate-200">
                <table class="min-w-full text-left text-sm">
                  <thead class="bg-[#f4f8f7] text-slate-500">
                    <tr>
                      <th class="px-4 py-3 font-semibold">Date</th>
                      <th class="px-4 py-3 font-semibold">Reference</th>
                      <th class="px-4 py-3 font-semibold">Amount</th>
                      <th class="px-4 py-3 font-semibold">Status</th>
                    </tr>
                  </thead>
                  <tbody class="divide-y divide-slate-200 bg-white text-slate-700">
                    <tr v-for="payment in financialHistory" :key="payment.reference">
                      <td class="px-4 py-3">{{ payment.date }}</td>
                      <td class="px-4 py-3 font-medium text-slate-900">{{ payment.reference }}</td>
                      <td class="px-4 py-3">{{ payment.amount }}</td>
                      <td class="px-4 py-3">
                        <span class="inline-flex rounded-full bg-emerald-50 px-2.5 py-1 text-xs font-semibold text-emerald-700">
                          {{ payment.status }}
                        </span>
                      </td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </section>
          </div>
        </section>
      </main>
    </div>
  </div>

  <div
    v-if="isTenantPopupOpen"
    class="fixed inset-0 z-50 flex items-center justify-center bg-slate-900/60 p-4"
    @click.self="closeTenantPopup"
  >
    <section class="w-full max-w-4xl overflow-hidden rounded-3xl bg-white shadow-2xl">
      <header class="border-b border-slate-100 p-5 sm:p-6 space-y-4">
        <div class="flex items-start justify-between gap-4">
          <div>
            <div class="flex flex-wrap items-center gap-3">
              <h2 class="text-2xl font-bold text-slate-900">Tenant</h2>
              <span class="rounded-full px-3 py-1 text-xs font-semibold bg-emerald-100 text-emerald-700">Active</span>
            </div>
            <p class="mt-2 text-sm text-slate-500">Tenant contact and lease overview</p>
          </div>

          <button
            type="button"
            class="inline-flex h-9 w-9 items-center justify-center rounded-lg border border-slate-300 text-lg font-semibold leading-none text-slate-600 hover:bg-slate-100"
            aria-label="Close tenant popup"
            @click="closeTenantPopup"
          >
            x
          </button>
        </div>

        <div class="flex flex-wrap gap-3">
          <button
            type="button"
            class="rounded-lg px-4 py-2 text-sm font-semibold transition-all"
            :class="modalView === 'edit' ? 'bg-[#00696b] text-white' : 'border border-slate-300 text-slate-700 hover:bg-slate-100'"
            @click="modalView = 'edit'"
          >
            Edit Profile
          </button>
          <button
            type="button"
            class="rounded-lg px-4 py-2 text-sm font-semibold transition-all"
            :class="modalView === 'lease' ? 'bg-[#00696b] text-white' : 'border border-slate-300 text-slate-700 hover:bg-slate-100'"
            @click="modalView = 'lease'"
          >
            Lease Information
          </button>
          <button
            type="button"
            class="rounded-lg px-4 py-2 text-sm font-semibold transition-all"
            :class="modalView === 'financial' ? 'bg-[#00696b] text-white' : 'border border-slate-300 text-slate-700 hover:bg-slate-100'"
            @click="modalView = 'financial'"
          >
            Financial History
          </button>
          <button
            v-if="modalView !== 'details'"
            type="button"
            class="rounded-lg px-4 py-2 text-sm font-semibold border border-slate-300 text-slate-700 hover:bg-slate-100 transition-all"
            @click="modalView = 'details'"
          >
            View Details
          </button>
        </div>
      </header>

      <div class="p-5 sm:p-6">
        <div v-if="modalView === 'details'" class="grid gap-4 lg:grid-cols-2">
          <section class="rounded-2xl border border-slate-100 bg-slate-50 p-4 space-y-4">
            <div>
              <h3 class="text-base font-semibold text-slate-900">Contact</h3>
              <p class="mt-1 text-sm text-slate-500">Primary contact details for the tenant.</p>
            </div>

            <dl class="grid grid-cols-1 sm:grid-cols-2 gap-3 text-sm">
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Email</dt>
                <dd class="mt-1 font-semibold text-slate-900">tenant@example.com</dd>
              </div>
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Phone No</dt>
                <dd class="mt-1 font-semibold text-slate-900">+254 712 345 678</dd>
              </div>
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Unit</dt>
                <dd class="mt-1 font-semibold text-slate-900">Unit 4B</dd>
              </div>
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Apartment</dt>
                <dd class="mt-1 font-semibold text-slate-900">A-14</dd>
              </div>
            </dl>
          </section>

          <section class="rounded-2xl border border-slate-100 bg-slate-50 p-4 space-y-4">
            <div>
              <h3 class="text-base font-semibold text-slate-900">Quick Lease</h3>
              <p class="mt-1 text-sm text-slate-500">Snapshot of lease and rent information.</p>
            </div>

            <div class="rounded-xl bg-white p-3 border border-slate-100">
              <p class="text-xs uppercase tracking-wide text-slate-500">Monthly Rent</p>
              <p class="mt-1 text-lg font-semibold text-[#00696b]">Ksh 4,500</p>
            </div>

            <div class="rounded-xl bg-white p-3 border border-slate-100">
              <p class="text-xs uppercase tracking-wide text-slate-500">Grace Period</p>
              <p class="mt-1 font-semibold text-emerald-600">3 Days</p>
            </div>
          </section>
        </div>

        <div v-else-if="modalView === 'lease'" class="grid gap-4 lg:grid-cols-2">
          <section class="rounded-2xl border border-slate-100 bg-slate-50 p-4 space-y-4">
            <div>
              <h3 class="text-base font-semibold text-slate-900">Lease Details</h3>
              <p class="mt-1 text-sm text-slate-500">Your current lease summary and payment terms.</p>
            </div>

            <dl class="grid grid-cols-1 sm:grid-cols-2 gap-3 text-sm">
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Lease Start</dt>
                <dd class="mt-1 font-semibold text-slate-900">Jan 01, 2026</dd>
              </div>
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Lease End</dt>
                <dd class="mt-1 font-semibold text-slate-900">Dec 31, 2026</dd>
              </div>
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Monthly Rent</dt>
                <dd class="mt-1 font-semibold text-slate-900">Ksh 4,500</dd>
              </div>
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Grace Period</dt>
                <dd class="mt-1 font-semibold text-emerald-600">3 Days</dd>
              </div>
            </dl>
          </section>
        </div>

        <div v-else-if="modalView === 'financial'">
          <div class="overflow-x-auto rounded-xl border border-slate-200">
            <table class="min-w-full text-left text-sm">
              <thead class="bg-[#f4f8f7] text-slate-500">
                <tr>
                  <th class="px-4 py-3 font-semibold">Date</th>
                  <th class="px-4 py-3 font-semibold">Reference</th>
                  <th class="px-4 py-3 font-semibold">Amount</th>
                  <th class="px-4 py-3 font-semibold">Status</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-slate-200 bg-white text-slate-700">
                <tr v-for="payment in financialHistory" :key="payment.reference">
                  <td class="px-4 py-3">{{ payment.date }}</td>
                  <td class="px-4 py-3 font-medium text-slate-900">{{ payment.reference }}</td>
                  <td class="px-4 py-3">{{ payment.amount }}</td>
                  <td class="px-4 py-3">
                    <span class="inline-flex rounded-full bg-emerald-50 px-2.5 py-1 text-xs font-semibold text-emerald-700">{{ payment.status }}</span>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <form v-else-if="modalView === 'edit'" class="grid gap-4 lg:grid-cols-2" @submit.prevent="saveTenantChanges">
          <section class="rounded-2xl border border-slate-100 bg-slate-50 p-4 space-y-4">
            <div>
              <h3 class="text-base font-semibold text-slate-900">Edit Profile</h3>
              <p class="mt-1 text-sm text-slate-500">Update tenant contact and unit assignment.</p>
            </div>

            <label class="block text-sm">
              <span class="mb-2 block font-medium text-slate-700">Email</span>
              <input v-model="tenantDraft.email" type="email" class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]" />
            </label>

            <label class="block text-sm">
              <span class="mb-2 block font-medium text-slate-700">Phone No</span>
              <input v-model="tenantDraft.phone" type="tel" class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]" />
            </label>

            <label class="block text-sm">
              <span class="mb-2 block font-medium text-slate-700">Apartment</span>
              <input v-model="tenantDraft.apartment" type="text" class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]" />
            </label>

            <label class="block text-sm">
              <span class="mb-2 block font-medium text-slate-700">Unit</span>
              <input v-model="tenantDraft.unit" type="text" class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]" />
            </label>

            <div class="flex gap-3 lg:col-span-2">
              <button type="button" class="w-full rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100" @click="modalView='details'">Cancel</button>
              <button type="submit" class="w-full rounded-lg bg-[#00696b] px-4 py-2 text-sm font-semibold text-white">Save Changes</button>
            </div>
          </section>
        </form>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import Sidebar from '~/components/shared/SharedSidebar.vue'

const financialHistory = [
  { date: 'Oct 01, 2023', reference: 'MP7XW92K01', amount: 'Ksh 4,500.00', status: 'Completed' },
  { date: 'Sep 01, 2023', reference: 'MP2RQ11L45', amount: 'Ksh 4,500.00', status: 'Completed' },
  { date: 'Aug 01, 2023', reference: 'MP8N55M92', amount: 'Ksh 4,500.00', status: 'Completed' }
]

const isTenantPopupOpen = ref(false)

const modalView = ref<'details' | 'lease' | 'financial' | 'edit'>('details')

const tenantDraft = ref({
  email: 'tenant@example.com',
  phone: '+254 712 345 678',
  apartment: 'A-14',
  unit: 'Unit 4B'
})

function saveTenantChanges() {
  // Placeholder: persist tenantDraft to API/store
  // For now just close and switch to details
  modalView.value = 'details'
}

function openTenantPopup() {
  isTenantPopupOpen.value = true
}

function closeTenantPopup() {
  isTenantPopupOpen.value = false
}
</script>
