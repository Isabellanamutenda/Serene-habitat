<template>
	<div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
		<!-- Sidebar -->
		<LandlordSidebar />

		<!-- Main content -->
		<div class="flex-1 flex flex-col">
			<!-- Page content -->
			<main class="p-4 sm:p-6 flex-1">
	<section id="overview" class="max-w-6xl mx-auto space-y-6 scroll-mt-6">
		<header>
			<h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Landlord Dashboard</h1>
			<p class="mt-2 text-slate-500 text-sm sm:text-base">Monitor unit readiness, maintenance tickets, service requests, and rent collections.</p>
		</header>

		<div class="grid grid-cols-1 md:grid-cols-3 gap-4">
			<article class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5">
				<p class="text-sm text-slate-500">Open Tickets</p>
				<p class="text-3xl font-semibold text-slate-800 mt-2">{{ openMaintenanceCount }}</p>
			</article>

			<article class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5">
				<p class="text-sm text-slate-500">Units Serviced</p>
				<p class="text-3xl font-semibold text-slate-800 mt-2">27</p>
			</article>

			<article class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5">
				<p class="text-sm text-slate-500">Escalations</p>
				<p class="text-3xl font-semibold text-slate-800 mt-2">2</p>
			</article>
		</div>

		<p v-if="landlordMessage" class="text-sm font-medium text-[#00696b]">{{ landlordMessage }}</p>

		<div class="grid grid-cols-1 xl:grid-cols-12 gap-6 items-start">
			<section id="tenants" class="xl:col-span-7 bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 scroll-mt-6">
				<header class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2">
					<div>
						<h2 class="text-xl font-semibold text-slate-900">Upcoming Payments</h2>
						<p class="text-sm text-slate-500">Payments due in the next 7 days.</p>
					</div>
				</header>

				<div class="mt-4 overflow-x-auto">
					<table class="min-w-full text-left text-sm">
						<thead>
							<tr class="border-b border-slate-100 text-slate-500">
								<th class="py-2 pr-4 font-medium">Tenant</th>
								<th class="py-2 pr-4 font-medium">Unit</th>
								<th class="py-2 pr-4 font-medium">Due date</th>
								<th class="py-2 pr-4 font-medium">Amount</th>
								<th class="py-2 font-medium text-right">Action</th>
							</tr>
						</thead>
						<tbody>
							<tr v-for="payment in upcomingPayments" :key="payment.id" class="border-b border-slate-50">
								<td class="py-3 pr-4 text-slate-700">{{ payment.tenant }}</td>
								<td class="py-3 pr-4 text-slate-700">{{ payment.unit }}</td>
								<td class="py-3 pr-4 text-slate-700">{{ payment.dueDate }}</td>
								<td class="py-3 pr-4 font-medium text-slate-900">{{ payment.amount }}</td>
								<td class="py-3 text-right">
									<button
										type="button"
										class="rounded-lg px-3 py-1.5 text-xs font-semibold transition-all"
										:class="payment.reminderSent ? 'bg-slate-100 text-slate-500 cursor-not-allowed' : 'bg-[#00696b] text-white hover:bg-[#004d4f]'"
										:disabled="payment.reminderSent"
										@click="sendReminder(payment.id)"
									>
										{{ payment.reminderSent ? 'Sent' : 'Send Reminder' }}
									</button>
								</td>
							</tr>
						</tbody>
					</table>
				</div>
			</section>

			<section id="maintenance" class="xl:col-span-5 bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 scroll-mt-6">
				<header>
					<h2 class="text-xl font-semibold text-slate-900">Maintenance Queue</h2>
					<p class="text-sm text-slate-500">Prioritize and resolve active maintenance requests.</p>
				</header>

				<ul class="mt-4 space-y-3">
					<li
						v-for="ticket in maintenanceQueue"
						:key="ticket.id"
						class="rounded-xl border border-slate-100 p-4"
					>
						<div class="flex items-start justify-between gap-4">
							<div>
								<p class="text-sm font-semibold text-slate-900">{{ ticket.unit }} • {{ ticket.issue }}</p>
								<p class="mt-1 text-xs text-slate-500">Age: {{ ticket.age }}</p>
							</div>
							<span
								class="rounded-full px-2.5 py-1 text-xs font-semibold"
								:class="ticket.priority === 'High' ? 'bg-rose-100 text-rose-700' : ticket.priority === 'Medium' ? 'bg-amber-100 text-amber-700' : 'bg-emerald-100 text-emerald-700'"
							>
								{{ ticket.priority }}
							</span>
						</div>

						<div class="mt-3 flex items-center justify-between">
							<p class="text-xs font-medium" :class="ticket.status === 'Resolved' ? 'text-emerald-700' : 'text-slate-500'">
								{{ ticket.status }}
							</p>
							<button
								type="button"
								class="rounded-lg px-3 py-1.5 text-xs font-semibold transition-all"
								:class="ticket.status === 'Resolved' ? 'bg-slate-100 text-slate-500 cursor-not-allowed' : 'bg-slate-900 text-white hover:bg-slate-700'"
								:disabled="ticket.status === 'Resolved'"
								@click="markResolved(ticket.id)"
							>
								{{ ticket.status === 'Resolved' ? 'Resolved' : 'Mark as Resolved' }}
							</button>
						</div>
					</li>
				</ul>
			</section>
		</div>

		<div id="units" class="grid grid-cols-1 xl:grid-cols-12 gap-6 items-start scroll-mt-6">
			<div class="xl:col-span-5">
				<LockControl unit="402" status="Locked" lastActivity="2 mins ago" />
			</div>
		</div>

		<div id="financials" class="scroll-mt-6">
			<PaymentHistory :transactions="transactions" />
		</div>
	</section>
			</main>
		</div>
	</div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import LandlordSidebar from '~/components/landlord/LandlordSidebar.vue'
import LockControl from '~/components/tenant/LockControl.vue'
import PaymentHistory from '~/components/tenant/PaymentHistory.vue'

const transactions = [
	{ date: 'Mar 29, 2026', ref: 'MP5ZT30A11', amount: 'Ksh 45,000.00', status: 'Completed' },
	{ date: 'Mar 27, 2026', ref: 'MP1HU72Q90', amount: 'Ksh 38,500.00', status: 'Completed' },
	{ date: 'Mar 25, 2026', ref: 'MP9LL14D06', amount: 'Ksh 41,250.00', status: 'Completed' }
]

type PaymentItem = {
	id: number
	tenant: string
	unit: string
	dueDate: string
	amount: string
	reminderSent: boolean
}

type MaintenanceItem = {
	id: number
	unit: string
	issue: string
	priority: 'High' | 'Medium' | 'Low'
	age: string
	status: 'Open' | 'Resolved'
}

const upcomingPayments = ref<PaymentItem[]>([
	{ id: 1, tenant: 'Ashley Tenant', unit: 'A-12', dueDate: 'Apr 10, 2026', amount: 'Ksh 45,000', reminderSent: false },
	{ id: 2, tenant: 'John Kariuki', unit: 'B-03', dueDate: 'Apr 11, 2026', amount: 'Ksh 38,500', reminderSent: false },
	{ id: 3, tenant: 'Faith Njeri', unit: 'C-05', dueDate: 'Apr 13, 2026', amount: 'Ksh 41,250', reminderSent: false }
])

const maintenanceQueue = ref<MaintenanceItem[]>([
	{ id: 1, unit: 'A-07', issue: 'Water leakage in kitchen sink', priority: 'High', age: '2 days', status: 'Open' },
	{ id: 2, unit: 'B-11', issue: 'Main door lock replacement', priority: 'Medium', age: '1 day', status: 'Open' },
	{ id: 3, unit: 'C-02', issue: 'Balcony light not working', priority: 'Low', age: '4 hours', status: 'Open' }
])

const landlordMessage = ref('')

const openMaintenanceCount = computed(() => maintenanceQueue.value.filter((ticket) => ticket.status === 'Open').length)

function sendReminder(paymentId: number) {
	upcomingPayments.value = upcomingPayments.value.map((payment) => {
		if (payment.id !== paymentId) return payment
		return { ...payment, reminderSent: true }
	})

	landlordMessage.value = 'Reminder sent successfully.'
}

function markResolved(ticketId: number) {
	maintenanceQueue.value = maintenanceQueue.value.map((ticket) => {
		if (ticket.id !== ticketId) return ticket
		return { ...ticket, status: 'Resolved' }
	})

	landlordMessage.value = 'Ticket marked as resolved.'
}
</script>
