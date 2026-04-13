<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <!-- Sidebar -->
    <Sidebar />

    <!-- Main content -->
    <div class="flex-1 flex flex-col">
      <!-- Page content -->
      <main class="p-4 sm:p-6 flex-1">
  <div class="max-w-7xl mx-auto space-y-12">
    <header class="flex flex-col xl:flex-row xl:items-start xl:justify-between gap-6">
      <div>
        <h1 class="text-4xl sm:text-5xl xl:text-6xl font-bold text-slate-900 tracking-tight">
          {{ greeting }}, <br />
          <span class="text-[#00696b]">Ashley.</span>
        </h1>
        <p class="mt-4 text-slate-400 max-w-md text-base sm:text-lg leading-relaxed">
          Your habitat is optimized for comfort. Everything is in order for your peaceful stay today.
        </p>
      </div>

      <div class="flex flex-col sm:flex-row gap-4 w-full xl:w-auto">
        <div class="w-full sm:w-auto bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6">
          <div class="text-xs font-semibold uppercase tracking-wide text-slate-500">Rent per Day</div>
          <div class="mt-2 text-slate-900 font-bold text-3xl leading-none">Ksh 4,500</div>
          <div class="mt-1 text-sm text-slate-500">Fully inclusive</div>
          <NuxtLink
            to="/tenant/details"
            class="mt-4 w-full inline-flex items-center justify-center bg-[#00696b] text-white py-2 rounded-lg font-semibold text-sm hover:bg-[#004d4f] transition-all"
          >
            View Details
          </NuxtLink>
        </div>
      </div>
    </header>

    <div class="space-y-8">
      <div class="grid grid-cols-1 xl:grid-cols-12 gap-6 items-start">
        <div class="xl:col-span-7">
          <Rewards :points="850" :goal="1000" streak="12 Months" />
        </div>
        <div class="xl:col-span-5 space-y-4">
          <button
            type="button"
            class="self-start inline-flex items-center justify-center min-w-28 bg-slate-900 text-white px-5 py-2 rounded-lg font-semibold text-sm hover:bg-slate-700 transition-all"
            @click="openPaymentModal"
          >
            Pay Rent
          </button>
          <GracePeriodCard :days-remaining="graceDaysRemaining" />
        </div>
      </div>
      <div id="transactions">
        <PaymentHistory :transactions="transactions" />
      </div>
    </div>
  </div>
      </main>
    </div>
  </div>

  <div
    v-if="isPaymentModalOpen"
    class="fixed inset-0 z-50 flex items-center justify-center bg-slate-900/60 p-4"
    @click.self="closePaymentModal"
  >
    <section class="w-full max-w-md rounded-2xl bg-white p-6 shadow-2xl">
      <header class="flex items-center justify-between gap-4">
        <h2 class="text-xl font-bold text-slate-900">Pay Rent</h2>
        <button
          type="button"
          aria-label="Close payment modal"
          class="inline-flex h-8 w-8 items-center justify-center rounded-lg border border-slate-300 text-lg font-semibold leading-none text-slate-600 hover:bg-slate-100"
          @click="closePaymentModal"
        >
          x
        </button>
      </header>

      <form class="mt-5 space-y-4" @submit.prevent="submitPayment">
        <div>
          <label for="payment-phone" class="mb-2 block text-sm font-medium text-slate-700">Phone number</label>
          <input
            id="payment-phone"
            v-model="paymentPhone"
            type="tel"
            placeholder="e.g. +254 712 345 678"
            class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
          />
        </div>

        <div>
          <label for="payment-amount" class="mb-2 block text-sm font-medium text-slate-700">Enter amount</label>
          <input
            id="payment-amount"
            v-model.number="paymentAmount"
            type="number"
            min="1"
            step="1"
            placeholder="e.g. 4500"
            class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
          />
        </div>

        <p v-if="paymentMessage" class="text-sm" :class="paymentStatus === 'error' ? 'text-rose-600' : 'text-[#00696b]'">
          {{ paymentMessage }}
        </p>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3 pt-1">
          <button
            type="button"
            class="w-full rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100 transition-all"
            @click="closePaymentModal"
          >
            Cancel
          </button>
          <button
            type="submit"
            class="w-full rounded-lg bg-[#00696b] px-4 py-2 text-sm font-semibold text-white hover:bg-[#004d4f] transition-all"
          >
            Pay Now
          </button>
        </div>
      </form>
    </section>
  </div>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref, computed } from 'vue'
import Sidebar from '~/components/shared/SharedSidebar.vue'
import GracePeriodCard from '~/components/landlord/GracePeriodCard.vue'
import Rewards from '~/components/tenant/Rewards.vue'
import PaymentHistory from '~/components/tenant/PaymentHistory.vue'

const graceDaysRemaining = 3

const transactions = [
  { date: 'Oct 01, 2023', ref: 'MP7XW92K01', amount: 'Ksh 4,500.00', status: 'Completed' },
  { date: 'Sep 30, 2023', ref: 'MP2RQ11L45', amount: 'Ksh 4,500.00', status: 'Completed' },
  { date: 'Sep 29, 2023', ref: 'MP8N55M92', amount: 'Ksh 4,500.00', status: 'Completed' }
]

const isPaymentModalOpen = ref(false)
const paymentPhone = ref('')
const paymentAmount = ref<number | null>(null)
const paymentMessage = ref('')
const paymentStatus = ref<'success' | 'error'>('success')

const currentHour = ref(new Date().getHours())
let clockTimer: ReturnType<typeof setInterval> | undefined

onMounted(() => {
  clockTimer = setInterval(() => {
    currentHour.value = new Date().getHours()
  }, 60000)
})

onBeforeUnmount(() => {
  if (clockTimer) clearInterval(clockTimer)
})

const greeting = computed(() => {
  if (currentHour.value < 12) return 'Good morning'
  if (currentHour.value < 18) return 'Good afternoon'
  return 'Good evening'
})

function openPaymentModal() {
  isPaymentModalOpen.value = true
  paymentPhone.value = ''
  paymentAmount.value = null
  paymentMessage.value = ''
}

function closePaymentModal() {
  isPaymentModalOpen.value = false
}

function submitPayment() {
  if (!paymentPhone.value.trim()) {
    paymentStatus.value = 'error'
    paymentMessage.value = 'Please enter a phone number.'
    return
  }

  if (!paymentAmount.value || paymentAmount.value <= 0) {
    paymentStatus.value = 'error'
    paymentMessage.value = 'Please enter a valid amount greater than 0.'
    return
  }

  paymentMessage.value = ''
  closePaymentModal()
}
</script>