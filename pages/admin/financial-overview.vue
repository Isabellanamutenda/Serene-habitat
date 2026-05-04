<template>
	<div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
		<AdminSidebar />

		<div class="flex-1 flex flex-col">
			<main class="p-4 sm:p-6 flex-1">
				<section class="max-w-7xl mx-auto space-y-6">
					<header>
						<h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Financial Overview</h1>
						<p class="mt-2 text-slate-500">Payment history, collections, and rental revenue metrics.</p>
					</header>

					<div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-4 gap-4">
						<article class="rounded-2xl border border-slate-100 bg-white shadow-sm p-5">
							<p class="text-xs uppercase tracking-wide text-slate-500">Total Payments</p>
							<p class="mt-2 text-3xl font-semibold text-slate-900">Ksh 24.5M</p>
						</article>
						<article class="rounded-2xl border border-slate-100 bg-white shadow-sm p-5">
							<p class="text-xs uppercase tracking-wide text-slate-500">Collected This Month</p>
							<p class="mt-2 text-3xl font-semibold text-emerald-700">Ksh 2.1M</p>
						</article>
						<article class="rounded-2xl border border-slate-100 bg-white shadow-sm p-5">
							<p class="text-xs uppercase tracking-wide text-slate-500">Expected Rent</p>
							<p class="mt-2 text-3xl font-semibold text-[#00696b]">Ksh 2.3M</p>
						</article>
						<article class="rounded-2xl border border-slate-100 bg-white shadow-sm p-5">
							<p class="text-xs uppercase tracking-wide text-slate-500">Outstanding</p>
							<p class="mt-2 text-3xl font-semibold text-rose-700">Ksh 0.8M</p>
						</article>
					</div>

					<section class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6">
						<h2 class="text-xl font-semibold text-slate-900">Revenue Notes</h2>
						<p class="mt-2 text-sm text-slate-500">Collections are healthy, but the outstanding balance should continue to be monitored across all properties.</p>
					</section>
				</section>
			</main>
		</div>
	</div>
</template>

<script setup lang="ts">
import AdminSidebar from '~/components/admin/AdminSidebar.vue'
</script>
