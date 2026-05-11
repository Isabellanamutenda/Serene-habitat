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
            <section class="w-full max-w-4xl overflow-hidden rounded-3xl bg-white shadow-2xl">
              <header class="border-b border-slate-100 p-5 sm:p-6 space-y-4">
                <div class="flex items-start justify-between gap-4">
                  <div>
                    <div class="flex flex-wrap items-center gap-3">
                      <h2 class="text-2xl font-bold text-slate-900">{{ selectedTenant.name }}</h2>
                      <span class="rounded-full px-3 py-1 text-xs font-semibold" :class="selectedTenant.status === 'Current' ? 'bg-emerald-100 text-emerald-700' : 'bg-amber-100 text-amber-700'">{{ selectedTenant.status }}</span>
                    </div>
                    <p class="mt-2 text-sm text-slate-500">Tenant contact and lease overview</p>
                  </div>

                  <button
                    type="button"
                    class="inline-flex h-9 w-9 items-center justify-center rounded-lg border border-slate-300 text-lg font-semibold leading-none text-slate-600 hover:bg-slate-100"
                    aria-label="Close tenant popup"
                    @click="closeTenantDetails"
                  >
                    x
                  </button>
                </div>

                <div class="flex flex-wrap gap-3">
                  <button
                    type="button"
                    class="rounded-lg px-4 py-2 text-sm font-semibold transition-all border border-slate-300 text-slate-700 hover:bg-slate-100"
                    @click="openEditProfile"
                  >
                    Edit Profile
                  </button>
                  <button
                    type="button"
                    class="rounded-lg px-4 py-2 text-sm font-semibold transition-all border border-slate-300 text-slate-700 hover:bg-slate-100"
                    @click="openMessageComposer"
                  >
                    Message
                  </button>
                </div>
              </header>

              <div class="p-5 sm:p-6">
                <div class="grid grid-cols-1 lg:grid-cols-3 gap-6 items-start">
                  <div class="lg:col-span-1 space-y-4">
                    <div class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                      <p class="text-xs uppercase tracking-wide text-slate-500">Email</p>
                      <p class="mt-1 font-semibold text-slate-900 break-all">{{ selectedTenant.email }}</p>
                    </div>

                    <div class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                      <p class="text-xs uppercase tracking-wide text-slate-500">Phone No</p>
                      <p class="mt-1 font-semibold text-slate-900">{{ selectedTenant.phone }}</p>
                    </div>

                    <div class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                      <p class="text-xs uppercase tracking-wide text-slate-500">Apartment</p>
                      <p class="mt-1 font-semibold text-slate-900">{{ selectedTenant.apartment || '—' }}</p>
                    </div>

                    <div class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                      <p class="text-xs uppercase tracking-wide text-slate-500">Unit</p>
                      <p class="mt-1 font-semibold text-slate-900">{{ selectedTenant.unit }}</p>
                    </div>
                  </div>

                  <section class="lg:col-span-2 grid grid-cols-1 gap-4">
                    <div v-if="modalView === 'details'" class="rounded-2xl border border-slate-100 bg-slate-50 p-4">
                      <h3 class="text-base font-semibold text-slate-900">Lease Information</h3>
                      <p class="mt-1 text-sm text-slate-500">Summary of the active lease.</p>

                      <div class="mt-4 grid grid-cols-1 sm:grid-cols-2 gap-3 text-sm">
                        <div class="rounded-xl bg-white p-3 border border-slate-100">
                          <dt class="text-xs uppercase tracking-wide text-slate-500">Lease Start</dt>
                          <dd class="mt-1 font-semibold text-slate-900">{{ selectedTenant.leaseStart }}</dd>
                        </div>
                        <div class="rounded-xl bg-white p-3 border border-slate-100">
                          <dt class="text-xs uppercase tracking-wide text-slate-500">Lease End</dt>
                          <dd class="mt-1 font-semibold text-slate-900">{{ selectedTenant.leaseEnd }}</dd>
                        </div>
                        <div class="rounded-xl bg-white p-3 border border-slate-100">
                          <dt class="text-xs uppercase tracking-wide text-slate-500">Monthly Rent</dt>
                          <dd class="mt-1 font-semibold text-slate-900">{{ selectedTenant.monthlyRent }}</dd>
                        </div>
                        <div class="rounded-xl bg-white p-3 border border-slate-100">
                          <dt class="text-xs uppercase tracking-wide text-slate-500">Status</dt>
                          <dd class="mt-1 font-semibold" :class="selectedTenant.status === 'Current' ? 'text-emerald-700' : 'text-amber-700'">{{ selectedTenant.status }}</dd>
                        </div>
                      </div>
                    </div>

                    <div v-if="modalView === 'details'" class="rounded-2xl border border-slate-100 bg-slate-50 p-4">
                      <h3 class="text-base font-semibold text-slate-900">Financial History</h3>
                      <p class="mt-1 text-sm text-slate-500">Recent payments for this tenant.</p>

                      <div class="mt-4 overflow-x-auto rounded-xl border border-slate-200">
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
                            <tr v-for="p in tenantFinancialsForSelected(selectedTenant)" :key="p.ref">
                              <td class="px-4 py-3">{{ p.date }}</td>
                              <td class="px-4 py-3 font-medium text-slate-900">{{ p.ref }}</td>
                              <td class="px-4 py-3">{{ p.amount }}</td>
                              <td class="px-4 py-3"><span class="inline-flex rounded-full bg-emerald-50 px-2.5 py-1 text-xs font-semibold text-emerald-700">{{ p.status }}</span></td>
                            </tr>
                          </tbody>
                        </table>
                      </div>
                    </div>

                    <form v-if="modalView === 'edit'" class="rounded-2xl border border-slate-100 bg-slate-50 p-4 grid gap-3" @submit.prevent="saveTenantEdit">
                      <h3 class="text-base font-semibold text-slate-900">Edit Tenant</h3>
                      <label class="block text-sm">
                        <span class="mb-2 block font-medium text-slate-700">Email</span>
                        <input v-model="tenantDraft.email" type="email" class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]" />
                      </label>
                      <label class="block text-sm">
                        <span class="mb-2 block font-medium text-slate-700">Phone</span>
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
                      <div class="flex gap-3">
                        <button type="button" class="w-full rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100" @click="cancelEdit">Cancel</button>
                        <button type="submit" class="w-full rounded-lg bg-[#00696b] px-4 py-2 text-sm font-semibold text-white">Save</button>
                      </div>
                    </form>

                    <div v-if="modalView === 'message'" class="rounded-2xl border border-slate-100 bg-slate-50 p-4">
                      <h3 class="text-base font-semibold text-slate-900">Message {{ selectedTenant.name }}</h3>
                      <p class="mt-1 text-sm text-slate-500">Send a quick message to this tenant.</p>
                      <label class="block mt-3 text-sm">
                        <textarea v-model="messageDraft" rows="4" class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"></textarea>
                      </label>
                      <div class="flex gap-3 mt-3">
                        <button type="button" class="w-full rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100" @click="cancelMessage">Cancel</button>
                        <button type="button" class="w-full rounded-lg bg-[#00696b] px-4 py-2 text-sm font-semibold text-white" @click="sendMessage">Send</button>
                      </div>
                    </div>
                  </section>
                </div>
              </div>
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
  apartment?: string
  nationalId: string
  monthlyRent: string
  leaseStart: string
  leaseEnd: string
  emergencyContact: string
}

const tenants = ref<Tenant[]>([
  {
    name: 'A. Mugo',
    unit: '101',
    phone: '+254 700 000 101',
    status: 'Current',
    email: 'amugo@example.com',
    apartment: 'A-1',
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
    apartment: 'B-2',
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
    apartment: 'C-3',
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
    apartment: 'D-4',
    nationalId: '31800265',
    monthlyRent: 'Ksh 39,000',
    leaseStart: 'Apr 1, 2026',
    leaseEnd: 'Mar 31, 2027',
    emergencyContact: 'S. Kilonzo (+254 744 660 412)'
  }
])

const selectedTenant = ref<Tenant | null>(null)

const modalView = ref<'details' | 'edit' | 'message'>('details')

const tenantDraft = ref<Partial<Tenant>>({})

const messageDraft = ref('')

function openEditProfile() {
  if (!selectedTenant.value) return
  modalView.value = 'edit'
  tenantDraft.value = { ...selectedTenant.value }
}

function cancelEdit() {
  modalView.value = 'details'
  tenantDraft.value = {}
}

function saveTenantEdit() {
  if (!selectedTenant.value) return
  const idx = tenants.value.findIndex(t => t.name === selectedTenant.value!.name)
  if (idx !== -1) {
    tenants.value[idx] = { ...tenants.value[idx], ...(tenantDraft.value as Tenant) }
    selectedTenant.value = tenants.value[idx]
  }
  modalView.value = 'details'
}

function openMessageComposer() {
  messageDraft.value = ''
  modalView.value = 'message'
}

function cancelMessage() {
  messageDraft.value = ''
  modalView.value = 'details'
}

function sendMessage() {
  // placeholder: send message via API
  // For now just log and close
  // eslint-disable-next-line no-console
  console.log('Send message to', selectedTenant.value?.name, messageDraft.value)
  messageDraft.value = ''
  modalView.value = 'details'
}

function openTenantDetails(tenant: Tenant) {
  selectedTenant.value = tenant
  modalView.value = 'details'
  tenantDraft.value = {}
}

function closeTenantDetails() {
  selectedTenant.value = null
}

const tenantFinancials = [
  { date: 'Apr 01, 2026', ref: 'TX1001', amount: 'Ksh 45,000', status: 'Completed' },
  { date: 'Mar 01, 2026', ref: 'TX0978', amount: 'Ksh 45,000', status: 'Completed' },
  { date: 'Feb 01, 2026', ref: 'TX0934', amount: 'Ksh 45,000', status: 'Completed' }
]

const tenantFinancialsForSelected = (tenant: Tenant | null) => {
  // placeholder: return tenant-specific records; currently returns the same sample list
  return tenantFinancials
}
</script>
