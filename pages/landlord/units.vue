<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <LandlordSidebar />

    <div class="flex-1 flex flex-col">
      <main class="p-4 sm:p-6 flex-1">
        <section class="max-w-6xl mx-auto space-y-6">
          <header class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
            <div>
              <h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Units</h1>
              <p class="mt-2 text-slate-500 text-sm sm:text-base">Track individual unit status, occupancy, and smart lock state.</p>
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
                <p class="text-xs uppercase tracking-wide text-slate-500">Total Units</p>
                <p class="mt-2 text-2xl font-semibold text-slate-900">{{ units.length }}</p>
              </article>
              <article class="rounded-l-xl rounded-r-[2rem] border border-slate-100 bg-slate-50 p-4">
                <p class="text-xs uppercase tracking-wide text-slate-500">Occupied</p>
                <p class="mt-2 text-2xl font-semibold text-emerald-700">{{ occupiedUnits }}</p>
              </article>
              <article class="rounded-l-xl rounded-r-[2rem] border border-slate-100 bg-slate-50 p-4">
                <p class="text-xs uppercase tracking-wide text-slate-500">Vacant</p>
                <p class="mt-2 text-2xl font-semibold text-amber-700">{{ vacantUnits }}</p>
              </article>
            </div>

            <div class="space-y-3">
              <h2 class="text-xl font-semibold text-slate-900">Unit Snapshot</h2>
              <div class="overflow-x-auto">
                <table class="min-w-full text-left text-sm">
                  <thead>
                    <tr class="border-b border-slate-100 text-slate-500">
                      <th class="py-2 pr-4 font-medium">Unit</th>
                      <th class="py-2 pr-4 font-medium">Tenant</th>
                      <th class="py-2 pr-4 font-medium">Status</th>
                      <th class="py-2 font-medium">Lock State</th>
                    </tr>
                  </thead>
                  <tbody>
                    <tr
                      v-for="unit in units"
                      :key="unit.unit"
                      class="border-b border-slate-50 cursor-pointer transition-colors hover:bg-slate-50"
                      role="button"
                      tabindex="0"
                      @click="openUnitDetails(unit)"
                      @keydown.enter.prevent="openUnitDetails(unit)"
                      @keydown.space.prevent="openUnitDetails(unit)"
                    >
                      <td class="py-3 pr-4 font-semibold text-slate-900">{{ unit.unit }}</td>
                      <td class="py-3 pr-4 text-slate-700">{{ unit.tenant }}</td>
                      <td class="py-3 pr-4 text-slate-700">{{ unit.status }}</td>
                      <td class="py-3 text-slate-700">{{ unit.lockState }}</td>
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

  <div
    v-if="selectedUnit"
    class="fixed inset-0 z-50 flex items-center justify-center bg-slate-900/60 p-4"
    @click.self="closeUnitDetails"
  >
    <section class="w-full max-w-4xl overflow-hidden rounded-3xl bg-white shadow-2xl">
      <header class="border-b border-slate-100 p-5 sm:p-6 space-y-4">
        <div class="flex items-start justify-between gap-4">
          <div>
            <div class="flex flex-wrap items-center gap-3">
              <h2 class="text-2xl font-bold text-slate-900">Unit {{ selectedUnit.unit }}</h2>
              <span
                class="rounded-full px-3 py-1 text-xs font-semibold"
                :class="selectedUnit.status === 'Occupied' ? 'bg-emerald-100 text-emerald-700' : 'bg-amber-100 text-amber-700'"
              >
                {{ selectedUnit.status }}
              </span>
            </div>
            <p class="mt-2 text-sm text-slate-500">
              {{ selectedUnit.status === 'Occupied' ? `Occupied by ${selectedUnit.tenant}` : 'This unit is currently vacant.' }}
            </p>
          </div>

          <button
            type="button"
            class="inline-flex h-9 w-9 items-center justify-center rounded-lg border border-slate-300 text-lg font-semibold leading-none text-slate-600 hover:bg-slate-100"
            aria-label="Close unit details"
            @click="closeUnitDetails"
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
            Edit Unit
          </button>
          <button
            type="button"
            class="rounded-lg px-4 py-2 text-sm font-semibold transition-all"
            :class="modalView === 'lease' ? 'bg-[#00696b] text-white' : 'border border-slate-300 text-slate-700 hover:bg-slate-100'"
            :disabled="!selectedUnit.leaseReference"
            :title="selectedUnit.leaseReference ? 'View lease details' : 'No active lease for vacant units'"
            @click="modalView = 'lease'"
          >
            View Lease
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
              <h3 class="text-base font-semibold text-slate-900">Occupancy Details</h3>
              <p class="mt-1 text-sm text-slate-500">Current living and rent status for this unit.</p>
            </div>

            <dl class="grid grid-cols-1 sm:grid-cols-2 gap-3 text-sm">
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Tenant</dt>
                <dd class="mt-1 font-semibold text-slate-900">{{ selectedUnit.status === 'Occupied' ? selectedUnit.tenant : 'No current tenant' }}</dd>
              </div>
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Started Living There</dt>
                <dd class="mt-1 font-semibold text-slate-900">{{ selectedUnit.moveInDate || 'N/A' }}</dd>
              </div>
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Rent Status</dt>
                <dd class="mt-1 font-semibold text-slate-900">{{ selectedUnit.rentStatus }}</dd>
              </div>
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Lock State</dt>
                <dd class="mt-1 font-semibold text-slate-900">{{ selectedUnit.lockState }}</dd>
              </div>
            </dl>

            <div class="rounded-xl bg-white p-3 border border-slate-100">
              <p class="text-xs uppercase tracking-wide text-slate-500">Monthly Rent</p>
              <p class="mt-1 text-lg font-semibold text-[#00696b]">{{ selectedUnit.monthlyRent }}</p>
            </div>
          </section>

          <section class="rounded-2xl border border-slate-100 bg-slate-50 p-4 space-y-4">
            <div>
              <h3 class="text-base font-semibold text-slate-900">Maintenance History</h3>
              <p class="mt-1 text-sm text-slate-500">Recent maintenance activity logged for the unit.</p>
            </div>

            <ul class="space-y-3">
              <li
                v-for="item in selectedUnit.maintenanceHistory"
                :key="`${selectedUnit.unit}-${item.date}-${item.issue}`"
                class="rounded-xl bg-white border border-slate-100 p-3"
              >
                <div class="flex items-start justify-between gap-4">
                  <div>
                    <p class="text-sm font-semibold text-slate-900">{{ item.issue }}</p>
                    <p class="mt-1 text-xs text-slate-500">{{ item.date }}</p>
                  </div>
                  <span
                    class="rounded-full px-2.5 py-1 text-xs font-semibold"
                    :class="item.status === 'Resolved' ? 'bg-emerald-100 text-emerald-700' : 'bg-amber-100 text-amber-700'"
                  >
                    {{ item.status }}
                  </span>
                </div>
              </li>
            </ul>
          </section>
        </div>

        <div v-else-if="modalView === 'lease'" class="grid gap-4 lg:grid-cols-2">
          <section class="rounded-2xl border border-slate-100 bg-slate-50 p-4 space-y-4">
            <div>
              <h3 class="text-base font-semibold text-slate-900">Lease Details</h3>
              <p class="mt-1 text-sm text-slate-500">Current lease information tied to this unit.</p>
            </div>

            <dl class="grid grid-cols-1 sm:grid-cols-2 gap-3 text-sm">
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Lease Reference</dt>
                <dd class="mt-1 font-semibold text-slate-900">{{ selectedUnit.leaseReference || 'No active lease' }}</dd>
              </div>
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Lease Start</dt>
                <dd class="mt-1 font-semibold text-slate-900">{{ selectedUnit.leaseStart || 'N/A' }}</dd>
              </div>
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Lease End</dt>
                <dd class="mt-1 font-semibold text-slate-900">{{ selectedUnit.leaseEnd || 'N/A' }}</dd>
              </div>
              <div class="rounded-xl bg-white p-3 border border-slate-100">
                <dt class="text-xs uppercase tracking-wide text-slate-500">Lease Status</dt>
                <dd class="mt-1 font-semibold text-slate-900">{{ selectedUnit.leaseStatus }}</dd>
              </div>
            </dl>

            <div class="rounded-xl bg-white p-3 border border-slate-100">
              <p class="text-xs uppercase tracking-wide text-slate-500">Rent Under Lease</p>
              <p class="mt-1 text-lg font-semibold text-[#00696b]">{{ selectedUnit.monthlyRent }}</p>
            </div>
          </section>

          <section class="rounded-2xl border border-slate-100 bg-slate-50 p-4 space-y-4">
            <div>
              <h3 class="text-base font-semibold text-slate-900">Lease Notes</h3>
              <p class="mt-1 text-sm text-slate-500">Useful context for lease review and renewal.</p>
            </div>

            <div class="rounded-xl bg-white border border-slate-100 p-4 text-sm text-slate-700 space-y-2">
              <p><span class="font-semibold text-slate-900">Tenant:</span> {{ selectedUnit.status === 'Occupied' ? selectedUnit.tenant : 'No active tenant' }}</p>
              <p><span class="font-semibold text-slate-900">Unit Condition:</span> {{ selectedUnit.status }}</p>
              <p><span class="font-semibold text-slate-900">Security Deposit:</span> {{ selectedUnit.securityDeposit }}</p>
            </div>
          </section>
        </div>

        <form v-else-if="unitDraft" class="grid gap-4 lg:grid-cols-2" @submit.prevent="saveUnitChanges">
          <section class="rounded-2xl border border-slate-100 bg-slate-50 p-4 space-y-4">
            <div>
              <h3 class="text-base font-semibold text-slate-900">Edit Unit</h3>
              <p class="mt-1 text-sm text-slate-500">Update the occupancy and lease information for this unit.</p>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
              <label class="block text-sm">
                <span class="mb-2 block font-medium text-slate-700">Unit Number</span>
                <input
                  v-model="unitDraft.unit"
                  type="text"
                  class="w-full rounded-lg border border-slate-300 bg-slate-100 px-3 py-2 text-sm text-slate-500"
                  disabled
                />
              </label>
              <label class="block text-sm">
                <span class="mb-2 block font-medium text-slate-700">Status</span>
                <select
                  v-model="unitDraft.status"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                >
                  <option value="Occupied">Occupied</option>
                  <option value="Vacant">Vacant</option>
                </select>
              </label>
              <label class="block text-sm">
                <span class="mb-2 block font-medium text-slate-700">Tenant</span>
                <input
                  v-model="unitDraft.tenant"
                  type="text"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                  placeholder="Tenant name"
                />
              </label>
              <label class="block text-sm">
                <span class="mb-2 block font-medium text-slate-700">Started Living There</span>
                <input
                  v-model="unitDraft.moveInDate"
                  type="text"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                  placeholder="e.g. 2025-11-01"
                />
              </label>
              <label class="block text-sm">
                <span class="mb-2 block font-medium text-slate-700">Rent Status</span>
                <select
                  v-model="unitDraft.rentStatus"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                >
                  <option value="Paid">Paid</option>
                  <option value="Due">Due</option>
                  <option value="Overdue">Overdue</option>
                  <option value="Vacant">Vacant</option>
                </select>
              </label>
              <label class="block text-sm">
                <span class="mb-2 block font-medium text-slate-700">Lock State</span>
                <select
                  v-model="unitDraft.lockState"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                >
                  <option value="Locked">Locked</option>
                  <option value="Unlocked">Unlocked</option>
                </select>
              </label>
              <label class="block text-sm">
                <span class="mb-2 block font-medium text-slate-700">Monthly Rent</span>
                <input
                  v-model="unitDraft.monthlyRent"
                  type="text"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                  placeholder="e.g. Ksh 45,000"
                />
              </label>
            </div>
          </section>

          <section class="rounded-2xl border border-slate-100 bg-slate-50 p-4 space-y-4">
            <div>
              <h3 class="text-base font-semibold text-slate-900">Current Maintenance History</h3>
              <p class="mt-1 text-sm text-slate-500">This list is shown for reference while you edit the unit.</p>
            </div>

            <ul class="space-y-3">
              <li
                v-for="item in unitDraft.maintenanceHistory"
                :key="`${unitDraft.unit}-${item.date}-${item.issue}`"
                class="rounded-xl bg-white border border-slate-100 p-3"
              >
                <p class="text-sm font-semibold text-slate-900">{{ item.issue }}</p>
                <p class="mt-1 text-xs text-slate-500">{{ item.date }} • {{ item.status }}</p>
              </li>
            </ul>
          </section>

          <div class="lg:col-span-2 flex flex-col sm:flex-row gap-3 justify-end pt-1">
            <button
              type="button"
              class="rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100 transition-all"
              @click="closeUnitDetails"
            >
              Cancel
            </button>
            <button
              type="submit"
              class="rounded-lg bg-[#00696b] px-4 py-2 text-sm font-semibold text-white hover:bg-[#004d4f] transition-all"
            >
              Save Changes
            </button>
          </div>
        </form>
      </div>
    </section>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import LandlordSidebar from '~/components/landlord/LandlordSidebar.vue'

type MaintenanceEntry = {
  date: string
  issue: string
  status: 'Resolved' | 'Pending'
}

type UnitRecord = {
  unit: string
  tenant: string
  status: 'Occupied' | 'Vacant'
  lockState: 'Locked' | 'Unlocked'
  moveInDate: string
  rentStatus: 'Paid' | 'Due' | 'Overdue' | 'Vacant'
  monthlyRent: string
  leaseReference: string
  leaseStart: string
  leaseEnd: string
  leaseStatus: string
  securityDeposit: string
  maintenanceHistory: MaintenanceEntry[]
}

const units = ref<UnitRecord[]>([
  {
    unit: '101',
    tenant: 'A. Mugo',
    status: 'Occupied',
    lockState: 'Locked',
    moveInDate: '2025-11-01',
    rentStatus: 'Paid',
    monthlyRent: 'Ksh 45,000',
    leaseReference: 'LS-101-2025',
    leaseStart: '2025-11-01',
    leaseEnd: '2026-10-31',
    leaseStatus: 'Active',
    securityDeposit: 'Ksh 45,000',
    maintenanceHistory: [
      { date: '2026-03-18', issue: 'Kitchen sink leak', status: 'Resolved' },
      { date: '2026-02-04', issue: 'Door sensor battery', status: 'Resolved' }
    ]
  },
  {
    unit: '102',
    tenant: 'Vacant',
    status: 'Vacant',
    lockState: 'Locked',
    moveInDate: '',
    rentStatus: 'Vacant',
    monthlyRent: 'Ksh 38,000',
    leaseReference: '',
    leaseStart: '',
    leaseEnd: '',
    leaseStatus: 'No active lease',
    securityDeposit: 'Ksh 0',
    maintenanceHistory: [
      { date: '2026-03-22', issue: 'Paint touch-up', status: 'Pending' }
    ]
  },
  {
    unit: '205',
    tenant: 'N. Wanjiku',
    status: 'Occupied',
    lockState: 'Unlocked',
    moveInDate: '2024-08-15',
    rentStatus: 'Due',
    monthlyRent: 'Ksh 41,000',
    leaseReference: 'LS-205-2024',
    leaseStart: '2024-08-15',
    leaseEnd: '2026-08-14',
    leaseStatus: 'Active',
    securityDeposit: 'Ksh 41,000',
    maintenanceHistory: [
      { date: '2026-03-11', issue: 'Bathroom tap replacement', status: 'Resolved' },
      { date: '2026-01-28', issue: 'Balcony light wiring', status: 'Resolved' }
    ]
  },
  {
    unit: '301',
    tenant: 'K. Otieno',
    status: 'Occupied',
    lockState: 'Locked',
    moveInDate: '2025-05-20',
    rentStatus: 'Overdue',
    monthlyRent: 'Ksh 47,500',
    leaseReference: 'LS-301-2025',
    leaseStart: '2025-05-20',
    leaseEnd: '2026-05-19',
    leaseStatus: 'Active',
    securityDeposit: 'Ksh 47,500',
    maintenanceHistory: [
      { date: '2026-04-02', issue: 'Lock calibration', status: 'Pending' },
      { date: '2026-02-16', issue: 'Water pressure check', status: 'Resolved' }
    ]
  }
])

const selectedUnitId = ref<string | null>(null)
const modalView = ref<'details' | 'edit' | 'lease'>('details')
const unitDraft = ref<UnitRecord | null>(null)

const selectedUnit = computed(() => {
  if (!selectedUnitId.value) return null
  return units.value.find((unit) => unit.unit === selectedUnitId.value) ?? null
})

const occupiedUnits = computed(() => units.value.filter((unit) => unit.status === 'Occupied').length)
const vacantUnits = computed(() => units.value.filter((unit) => unit.status === 'Vacant').length)

function openUnitDetails(unit: UnitRecord) {
  selectedUnitId.value = unit.unit
  modalView.value = 'details'
  unitDraft.value = {
    ...unit,
    maintenanceHistory: unit.maintenanceHistory.map((entry) => ({ ...entry }))
  }
}

function closeUnitDetails() {
  selectedUnitId.value = null
  modalView.value = 'details'
  unitDraft.value = null
}

function saveUnitChanges() {
  if (!unitDraft.value) return

  const updatedUnit = {
    ...unitDraft.value,
    tenant: unitDraft.value.status === 'Vacant' ? 'Vacant' : unitDraft.value.tenant,
    moveInDate: unitDraft.value.status === 'Vacant' ? '' : unitDraft.value.moveInDate,
    rentStatus: unitDraft.value.status === 'Vacant' ? 'Vacant' : unitDraft.value.rentStatus,
    leaseReference: unitDraft.value.status === 'Vacant' ? '' : unitDraft.value.leaseReference,
    leaseStart: unitDraft.value.status === 'Vacant' ? '' : unitDraft.value.leaseStart,
    leaseEnd: unitDraft.value.status === 'Vacant' ? '' : unitDraft.value.leaseEnd,
    leaseStatus: unitDraft.value.status === 'Vacant' ? 'No active lease' : unitDraft.value.leaseStatus
  }

  const unitIndex = units.value.findIndex((unit) => unit.unit === updatedUnit.unit)

  if (unitIndex !== -1) {
    units.value[unitIndex] = {
      ...updatedUnit,
      maintenanceHistory: updatedUnit.maintenanceHistory.map((entry) => ({ ...entry }))
    }
  }

  unitDraft.value = {
    ...updatedUnit,
    maintenanceHistory: updatedUnit.maintenanceHistory.map((entry) => ({ ...entry }))
  }
  selectedUnitId.value = updatedUnit.unit
  modalView.value = 'details'
}
</script>
