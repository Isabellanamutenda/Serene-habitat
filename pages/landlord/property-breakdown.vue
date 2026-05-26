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
                    <td class="py-3 pr-4 font-semibold text-slate-900">
                      <button
                        class="text-sky-600 hover:underline font-semibold"
                        @click="openProperty(property)"
                      >
                        {{ property.name }}
                      </button>
                    </td>
                    <td class="py-3 pr-4 text-slate-700">{{ property.location }}</td>
                    <td class="py-3 pr-4 text-slate-700">{{ property.units }}</td>
                    <td class="py-3 font-semibold text-[#00696b]">{{ property.rent }}</td>
                  </tr>
                </tbody>
              </table>
            </div>
          </section>

          <div
            v-if="selectedProperty"
            class="fixed inset-0 z-50 bg-slate-900/50 backdrop-blur-sm flex items-center justify-center p-4"
            @click="closeProperty"
          >
            <div class="w-full max-w-3xl rounded-2xl bg-white text-slate-800 p-6 shadow-2xl" @click.stop>
              <div class="flex items-start justify-between gap-4">
                <div>
                  <h2 class="text-2xl font-bold text-slate-900">{{ selectedProperty.name }}</h2>
                  <p class="mt-1 text-sm text-slate-500">{{ selectedProperty.location }} · {{ selectedProperty.units }} units</p>
                </div>
                <button
                  class="text-slate-500 hover:text-slate-900 transition-colors"
                  aria-label="Close property details"
                  @click="closeProperty"
                >
                  ✕
                </button>
              </div>

              <div class="mt-5 grid grid-cols-1 sm:grid-cols-3 gap-4">
                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Total Rooms</p>
                  <p class="mt-2 text-2xl font-semibold text-slate-900">{{ selectedProperty.details.totalRooms }}</p>
                </article>
                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Bedrooms</p>
                  <p class="mt-2 text-2xl font-semibold text-slate-900">{{ selectedProperty.details.bedrooms }}</p>
                </article>
                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Ensuite Units</p>
                  <p class="mt-2 text-2xl font-semibold text-slate-900">{{ selectedProperty.details.ensuiteUnits }}</p>
                </article>
              </div>

              <div class="mt-6 grid grid-cols-1 lg:grid-cols-2 gap-4">
                <section class="rounded-xl border border-slate-100 p-4">
                  <h3 class="text-lg font-semibold text-slate-900">Unit Breakdown</h3>
                  <ul class="mt-3 space-y-2 text-sm text-slate-700">
                    <li v-for="item in selectedProperty.details.roomTypes" :key="item.label" class="flex items-center justify-between border-b border-slate-100 pb-2 last:border-b-0 last:pb-0">
                      <span>{{ item.label }}</span>
                      <span class="font-semibold text-slate-900">{{ item.count }}</span>
                    </li>
                  </ul>
                </section>

                <section class="rounded-xl border border-slate-100 p-4">
                  <h3 class="text-lg font-semibold text-slate-900">More Details</h3>
                  <dl class="mt-3 space-y-3 text-sm">
                    <div class="flex items-center justify-between gap-4 border-b border-slate-100 pb-2">
                      <dt class="text-slate-500">Average Rent per Unit</dt>
                      <dd class="font-semibold text-slate-900">{{ selectedProperty.details.averageRent }}</dd>
                    </div>
                    <div class="flex items-center justify-between gap-4 border-b border-slate-100 pb-2">
                      <dt class="text-slate-500">Occupied Units</dt>
                      <dd class="font-semibold text-slate-900">{{ selectedProperty.details.occupiedUnits }}</dd>
                    </div>
                    <div class="flex items-center justify-between gap-4">
                      <dt class="text-slate-500">Vacant Units</dt>
                      <dd class="font-semibold text-slate-900">{{ selectedProperty.details.vacantUnits }}</dd>
                    </div>
                  </dl>
                </section>
              </div>
            </div>
          </div>
        </section>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import LandlordSidebar from '~/components/landlord/LandlordSidebar.vue'

interface PropertyDetail {
  totalRooms: number
  bedrooms: number
  ensuiteUnits: number
  roomTypes: Array<{ label: string; count: number }>
  averageRent: string
  occupiedUnits: number
  vacantUnits: number
}

interface PropertyRow {
  name: string
  location: string
  units: number
  rent: string
  details: PropertyDetail
}

const properties: PropertyRow[] = [
  {
    name: 'Riverside Suites',
    location: 'Westlands',
    units: 18,
    rent: 'Ksh 1.2M',
    details: {
      totalRooms: 54,
      bedrooms: 36,
      ensuiteUnits: 12,
      roomTypes: [
        { label: 'Studio units', count: 6 },
        { label: '1-bedroom units', count: 8 },
        { label: '2-bedroom units', count: 4 }
      ],
      averageRent: 'Ksh 66,700',
      occupiedUnits: 16,
      vacantUnits: 2
    }
  },
  {
    name: 'Downtown Plaza',
    location: 'CBD',
    units: 22,
    rent: 'Ksh 1.5M',
    details: {
      totalRooms: 66,
      bedrooms: 44,
      ensuiteUnits: 15,
      roomTypes: [
        { label: 'Studio units', count: 4 },
        { label: '1-bedroom units', count: 12 },
        { label: '2-bedroom units', count: 6 }
      ],
      averageRent: 'Ksh 68,200',
      occupiedUnits: 20,
      vacantUnits: 2
    }
  },
  {
    name: 'Green Valley',
    location: 'Kileleshwa',
    units: 14,
    rent: 'Ksh 980K',
    details: {
      totalRooms: 42,
      bedrooms: 28,
      ensuiteUnits: 10,
      roomTypes: [
        { label: 'Studio units', count: 3 },
        { label: '1-bedroom units', count: 7 },
        { label: '2-bedroom units', count: 4 }
      ],
      averageRent: 'Ksh 70,000',
      occupiedUnits: 13,
      vacantUnits: 1
    }
  }
]

const selectedProperty = ref<PropertyRow | null>(null)

const openProperty = (property: PropertyRow) => {
  selectedProperty.value = property
}

const closeProperty = () => {
  selectedProperty.value = null
}
</script>
