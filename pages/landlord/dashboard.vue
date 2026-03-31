<template>
	<div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
		<!-- Sidebar -->
		<Sidebar />

		<!-- Main content -->
		<div class="flex-1 flex flex-col">
			<!-- Page content -->
			<main class="p-4 sm:p-6 flex-1">
	<section class="max-w-6xl mx-auto space-y-6">
		<header>
			<h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Landlord Dashboard</h1>
			<p class="mt-2 text-slate-500 text-sm sm:text-base">Monitor unit readiness, maintenance tickets, service requests, and rent collections.</p>
		</header>

		<div class="grid grid-cols-1 md:grid-cols-3 gap-4">
			<article class="card">
				<p class="text-sm text-slate-500">Open Tickets</p>
				<p class="text-3xl font-semibold text-slate-800 mt-2">12</p>
			</article>

			<article class="card">
				<p class="text-sm text-slate-500">Units Serviced</p>
				<p class="text-3xl font-semibold text-slate-800 mt-2">27</p>
			</article>

			<article class="card">
				<p class="text-sm text-slate-500">Escalations</p>
				<p class="text-3xl font-semibold text-slate-800 mt-2">2</p>
			</article>
		</div>

		<div class="grid grid-cols-1 xl:grid-cols-12 gap-6 items-start">
			<div class="xl:col-span-5">
				<LockControl unit="402" status="Locked" lastActivity="2 mins ago" />
			</div>
		</div>

		<div id="transactions">
			<PaymentHistory :transactions="transactions" />
		</div>
	</section>
			</main>
		</div>
	</div>
</template>

<script setup lang="ts">
import Sidebar from '~/components/shared/SharedSidebar.vue'
import LockControl from '~/components/tenant/LockControl.vue'
import PaymentHistory from '~/components/tenant/PaymentHistory.vue'

const transactions = [
	{ date: 'Mar 29, 2026', ref: 'MP5ZT30A11', amount: 'Ksh 45,000.00', status: 'Completed' },
	{ date: 'Mar 27, 2026', ref: 'MP1HU72Q90', amount: 'Ksh 38,500.00', status: 'Completed' },
	{ date: 'Mar 25, 2026', ref: 'MP9LL14D06', amount: 'Ksh 41,250.00', status: 'Completed' }
]
</script>
