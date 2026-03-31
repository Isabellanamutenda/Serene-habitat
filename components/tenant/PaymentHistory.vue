<template>
  <SharedCard title="Recent Transactions">
    <table class="w-full text-sm text-left text-gray-300">
      <thead class="text-gray-400">
        <tr>
          <th class="py-2">DATE</th>
          <th class="py-2">REFERENCE ID</th>
          <th class="py-2">AMOUNT</th>
          <th class="py-2">STATUS</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="tx in transactions"
          :key="tx.ref"
          class="border-t border-gray-700 cursor-pointer hover:bg-gray-800/40 transition-colors"
          :class="{ 'bg-gray-800/50': activeTransaction?.ref === tx.ref }"
          @click="toggleTransaction(tx)"
        >
            <td class="py-2">{{ tx.date }}</td>
            <td class="py-2">{{ tx.ref }}</td>
            <td class="py-2">{{ tx.amount }}</td>
            <td class="py-2 text-green-400">{{ tx.status }}</td>
        </tr>
      </tbody>
    </table>

    <div
      v-if="activeTransaction"
      class="fixed inset-0 z-50 bg-slate-900/50 backdrop-blur-sm flex items-center justify-center p-4"
      @click="closeTransaction"
    >
      <div
        class="w-full max-w-md rounded-xl bg-white text-slate-800 p-5 shadow-2xl"
        @click.stop
      >
        <div class="flex items-start justify-between gap-4">
          <h4 class="text-lg font-semibold">Transaction Details</h4>
          <button
            class="text-slate-500 hover:text-slate-800 transition-colors"
            aria-label="Close transaction popup"
            @click="closeTransaction"
          >
            ✕
          </button>
        </div>

        <div class="mt-4 space-y-2 text-sm">
          <p><strong>Date:</strong> {{ activeTransaction.date }}</p>
          <p><strong>Reference:</strong> {{ activeTransaction.ref }}</p>
          <p><strong>Amount:</strong> {{ activeTransaction.amount }}</p>
          <p><strong>Status:</strong> {{ activeTransaction.status }}</p>
        </div>
      </div>
    </div>
  </SharedCard>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import SharedCard from '~/components/shared/SharedCard.vue'

interface Transaction {
  date: string
  ref: string
  amount: string
  status: string
}

defineProps<{
  transactions: Transaction[]
}>()

const activeTransaction = ref<Transaction | null>(null)

const toggleTransaction = (transaction: Transaction) => {
  activeTransaction.value = activeTransaction.value?.ref === transaction.ref ? null : transaction
}

const closeTransaction = () => {
  activeTransaction.value = null
}
</script>
