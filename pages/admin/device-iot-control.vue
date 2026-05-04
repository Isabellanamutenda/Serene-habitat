<template>
	<div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
		<AdminSidebar />

		<div class="flex-1 flex flex-col">
			<main class="p-4 sm:p-6 flex-1">
				<section class="max-w-7xl mx-auto space-y-6">
					<header>
						<h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Device/IoT Control</h1>
						<p class="mt-2 text-slate-500">Real-time device connectivity and status across all properties.</p>
					</header>

					<div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
						<article class="rounded-2xl border border-slate-100 bg-white shadow-sm p-5">
							<p class="text-xs uppercase tracking-wide text-slate-500">Total Devices</p>
							<p class="mt-2 text-3xl font-semibold text-slate-900">6</p>
						</article>
						<article class="rounded-2xl border border-slate-100 bg-white shadow-sm p-5">
							<p class="text-xs uppercase tracking-wide text-slate-500">Online Devices</p>
							<p class="mt-2 text-3xl font-semibold text-emerald-700">4</p>
						</article>
						<article class="rounded-2xl border border-slate-100 bg-white shadow-sm p-5">
							<p class="text-xs uppercase tracking-wide text-slate-500">Offline Devices</p>
							<p class="mt-2 text-3xl font-semibold text-rose-700">2</p>
						</article>
					</div>

					<section class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6">
						<div class="overflow-x-auto">
							<table class="min-w-full text-left text-sm">
								<thead>
									<tr class="border-b border-slate-100 text-slate-500">
										<th class="py-2 pr-4 font-medium">Device ID</th>
										<th class="py-2 pr-4 font-medium">Status</th>
										<th class="py-2 pr-4 font-medium">Last Seen At</th>
										<th class="py-2 pr-4 font-medium">Unit Number</th>
										<th class="py-2 font-medium">Property Name</th>
									</tr>
								</thead>
								<tbody>
									<tr class="border-b border-slate-50">
										<td class="py-3 pr-4 font-semibold text-slate-900">DV-001</td>
										<td class="py-3 pr-4 text-emerald-700 font-semibold">Online</td>
										<td class="py-3 pr-4 text-slate-700">2 mins ago</td>
										<td class="py-3 pr-4 text-slate-700">101</td>
										<td class="py-3 text-slate-700">Riverside Suites</td>
									</tr>
									<tr class="border-b border-slate-50">
										<td class="py-3 pr-4 font-semibold text-slate-900">DV-003</td>
										<td class="py-3 pr-4 text-rose-700 font-semibold">Offline</td>
										<td class="py-3 pr-4 text-slate-700">1 hr ago</td>
										<td class="py-3 pr-4 text-slate-700">412</td>
										<td class="py-3 text-slate-700">Downtown Plaza</td>
									</tr>
								</tbody>
							</table>
						</div>
					</section>
				</section>
			</main>
		</div>
	</div>
</template>

<script setup lang="ts">
import AdminSidebar from '~/components/admin/AdminSidebar.vue'
</script>
