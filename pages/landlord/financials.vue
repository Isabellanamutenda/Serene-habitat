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
                      <td class="py-3 pr-4 font-semibold text-slate-900">
                        <button
                          class="text-sky-600 hover:underline font-semibold"
                          @click="openTenant(payment)"
                        >
                          {{ payment.tenant }}
                        </button>
                      </td>
                      <td class="py-3 pr-4 text-emerald-700 font-semibold">{{ payment.amount }}</td>
                      <td class="py-3 text-slate-700">{{ payment.date }}</td>
                    </tr>
                  </tbody>
                </table>
              </div>
            </div>
            
            <!-- Tenant transactions modal -->
            <div
              v-if="selectedTenant"
              class="fixed inset-0 z-50 bg-slate-900/50 backdrop-blur-sm flex items-center justify-center p-4"
              @click="closeTenant"
            >
              <div class="w-full max-w-2xl rounded-xl bg-white text-slate-800 p-6 shadow-2xl" @click.stop>
                <div class="flex items-center justify-between">
                  <h3 class="text-lg font-semibold">{{ selectedTenant.tenant }} — Recent Payments</h3>
                  <button class="text-slate-500 hover:text-slate-800" @click="closeTenant">✕</button>
                </div>

                <div class="mt-4 grid grid-cols-1 lg:grid-cols-2 gap-4">
                  <SharedCard title="Last 3 Transactions">
                    <table class="w-full text-sm text-left text-slate-700">
                      <thead class="text-slate-500">
                        <tr>
                          <th class="py-2">Date</th>
                          <th class="py-2">Amount</th>
                          <th class="py-2">Method</th>
                        </tr>
                      </thead>
                      <tbody>
                        <tr v-for="tx in lastThree" :key="tx.date" class="border-t border-slate-100">
                          <td class="py-2">{{ tx.date }}</td>
                          <td class="py-2">{{ tx.amount }}</td>
                          <td class="py-2">{{ tx.method }}</td>
                        </tr>
                      </tbody>
                    </table>
                  </SharedCard>

                  <SharedCard title="Payment Methods Summary">
                    <ul class="text-sm space-y-2">
                      <li v-for="m in paymentMethodsSummary" :key="m.method" class="flex items-center justify-between">
                        <span>{{ m.method }}</span>
                        <span class="text-slate-600">{{ m.count }} / 3</span>
                      </li>
                    </ul>
                  </SharedCard>
                </div>
              </div>
            </div>
          </section>
        </section>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import LandlordSidebar from '~/components/landlord/LandlordSidebar.vue'
import SharedCard from '~/components/shared/SharedCard.vue'

interface Transaction {
  date: string
  amount: string
  method: string
}

interface PaymentRow {
  tenant: string
  amount: string
  date: string
  transactions: Transaction[]
}

const payments: PaymentRow[] = [
  {
    tenant: 'A. Mugo',
    amount: 'Ksh 45,000',
    date: 'Apr 20, 2026',
    transactions: [
      { date: 'Apr 20, 2026', amount: 'Ksh 45,000', method: 'M-Pesa' },
      { date: 'Mar 20, 2026', amount: 'Ksh 45,000', method: 'Card' },
      { date: 'Feb 20, 2026', amount: 'Ksh 45,000', method: 'M-Pesa' }
    ]
  },
  {
    tenant: 'N. Wanjiku',
    amount: 'Ksh 38,500',
    date: 'Apr 18, 2026',
    transactions: [
      { date: 'Apr 18, 2026', amount: 'Ksh 38,500', method: 'Bank Transfer' },
      { date: 'Mar 18, 2026', amount: 'Ksh 38,500', method: 'Bank Transfer' },
      { date: 'Feb 18, 2026', amount: 'Ksh 38,500', method: 'M-Pesa' }
    ]
  },
  {
    tenant: 'K. Otieno',
    amount: 'Ksh 41,250',
    date: 'Apr 17, 2026',
    transactions: [
      { date: 'Apr 17, 2026', amount: 'Ksh 41,250', method: 'Card' },
      { date: 'Mar 17, 2026', amount: 'Ksh 41,250', method: 'Card' },
      { date: 'Feb 17, 2026', amount: 'Ksh 41,250', method: 'Card' }
    ]
  }
]

const selectedTenant = ref<PaymentRow | null>(null)

const openTenant = (row: PaymentRow) => {
  selectedTenant.value = row
}

const closeTenant = () => (selectedTenant.value = null)

const lastThree = computed(() => selectedTenant.value ? selectedTenant.value.transactions.slice(0, 3) : [])

const paymentMethodsSummary = computed(() => {
  if (!selectedTenant.value) return [] as { method: string; count: number }[]
  const map = new Map<string, number>()
  selectedTenant.value.transactions.slice(0, 3).forEach(t => {
    map.set(t.method, (map.get(t.method) || 0) + 1)
  })
  return Array.from(map.entries()).map(([method, count]) => ({ method, count }))
})
</script>
