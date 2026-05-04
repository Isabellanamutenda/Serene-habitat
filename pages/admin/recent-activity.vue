<template>
	<div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
		<AdminSidebar />

		<div class="flex-1 flex flex-col">
			<main class="p-4 sm:p-6 flex-1">
				<section class="max-w-7xl mx-auto space-y-6">
					<header>
						<h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Recent Activity</h1>
						<p class="mt-2 text-slate-500">Latest payments and device command actions across the platform.</p>
					</header>

					<div class="grid grid-cols-1 xl:grid-cols-2 gap-6">
						<section class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6">
							<h2 class="text-xl font-semibold text-slate-900">Latest Payments</h2>
							<div class="mt-4 overflow-x-auto">
								<table class="min-w-full text-left text-sm">
									<thead>
										<tr class="border-b border-slate-100 text-slate-500">
											<th class="py-2 pr-4 font-medium">Payment Ref</th>
											<th class="py-2 pr-4 font-medium">Tenant</th>
											<th class="py-2 pr-4 font-medium">Amount</th>
											<th class="py-2 font-medium">Time</th>
										</tr>
									</thead>
									<tbody>
										<tr class="border-b border-slate-50">
											<td class="py-2 pr-4 font-semibold text-slate-900">MPX00172</td>
											<td class="py-2 pr-4 text-slate-700">A. Mugo</td>
											<td class="py-2 pr-4 text-emerald-700 font-semibold">Ksh 45,000</td>
											<td class="py-2 text-slate-500">2 min ago</td>
										</tr>
									</tbody>
								</table>
							</div>
						</section>

						<section class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6">
							<h2 class="text-xl font-semibold text-slate-900">Latest Device Commands</h2>
							<div class="mt-4 overflow-x-auto">
								<table class="min-w-full text-left text-sm">
									<thead>
										<tr class="border-b border-slate-100 text-slate-500">
											<th class="py-2 pr-4 font-medium">Command</th>
											<th class="py-2 pr-4 font-medium">Device ID</th>
											<th class="py-2 pr-4 font-medium">Admin</th>
											<th class="py-2 font-medium">Time</th>
										</tr>
									</thead>
									<tbody>
										<tr class="border-b border-slate-50">
											<td class="py-2 pr-4 font-semibold text-slate-900">Unlock Door</td>
											<td class="py-2 pr-4 text-slate-700">DV-001</td>
											<td class="py-2 pr-4 text-slate-700">Admin User</td>
											<td class="py-2 text-slate-500">1 min ago</td>
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
</template>

<script setup lang="ts">
import AdminSidebar from '~/components/admin/AdminSidebar.vue'
</script>
