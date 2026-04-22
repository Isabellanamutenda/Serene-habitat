<template>
	<div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
		<AdminSidebar />

		<div class="flex-1 flex flex-col">
			<main class="p-4 sm:p-6 flex-1">
				<section class="max-w-7xl mx-auto space-y-6">
					<header>
						<h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Admin Dashboard</h1>
						<p class="mt-2 text-slate-500">Centralized visibility and control for properties, finance, and smart infrastructure.</p>
					</header>

					<section id="system-overview" class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-4 scroll-mt-6">
						<div>
							<h2 class="text-xl font-semibold text-slate-900">System Overview</h2>
							<p class="text-sm text-slate-500">Platform-wide entities and resource allocation summary.</p>
						</div>

						<div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-5 gap-4">
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Total Organizations</p>
								<p class="mt-2 text-2xl font-semibold text-slate-900">8</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Total Landlords</p>
								<p class="mt-2 text-2xl font-semibold text-slate-900">42</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Total Units</p>
								<p class="mt-2 text-2xl font-semibold text-emerald-700">487</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Total Properties</p>
								<p class="mt-2 text-2xl font-semibold text-slate-900">56</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Total Tenants</p>
								<p class="mt-2 text-2xl font-semibold text-[#00696b]">478</p>
							</article>
						</div>
					</section>

					<section id="financial-overview" class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-4 scroll-mt-6">
						<div>
							<h2 class="text-xl font-semibold text-slate-900">Financial Overview</h2>
							<p class="text-sm text-slate-500">Payment history, collections, and rental revenue metrics.</p>
						</div>

						<div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-4 gap-4">
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Total Payments (All Time)</p>
								<p class="mt-2 text-2xl font-semibold text-slate-900">Ksh 24.5M</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Collected This Month</p>
								<p class="mt-2 text-2xl font-semibold text-emerald-700">Ksh 2.1M</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Total Expected Rent</p>
								<p class="mt-2 text-2xl font-semibold text-[#00696b]">Ksh 2.3M</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Total Outstanding</p>
								<p class="mt-2 text-2xl font-semibold text-rose-700">Ksh 0.8M</p>
							</article>
						</div>
					</section>

					<section id="device-iot-control" class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-4 scroll-mt-6">
						<div>
							<h2 class="text-xl font-semibold text-slate-900">Device/IoT Control</h2>
							<p class="text-sm text-slate-500">Real-time device connectivity and status across all properties.</p>
						</div>

						<div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Total Devices</p>
								<p class="mt-2 text-2xl font-semibold text-slate-900">{{ deviceList.length }}</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Online Devices</p>
								<p class="mt-2 text-2xl font-semibold text-emerald-700">{{ onlineDeviceCount }}</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Offline Devices</p>
								<p class="mt-2 text-2xl font-semibold text-rose-700">{{ offlineDeviceCount }}</p>
							</article>
						</div>

						<div class="overflow-x-auto mt-4">
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
									<tr v-for="device in deviceList" :key="device.id" class="border-b border-slate-50">
										<td class="py-3 pr-4 font-semibold text-slate-900">{{ device.id }}</td>
										<td class="py-3 pr-4">
											<span class="rounded-full px-2.5 py-1 text-xs font-semibold" :class="device.status === 'Online' ? 'bg-emerald-100 text-emerald-700' : 'bg-rose-100 text-rose-700'">
												{{ device.status }}
											</span>
										</td>
										<td class="py-3 pr-4 text-slate-700">{{ device.lastSeenAt }}</td>
										<td class="py-3 pr-4 text-slate-700">{{ device.unitNumber }}</td>
										<td class="py-3 text-slate-700">{{ device.propertyName }}</td>
									</tr>
								</tbody>
							</table>
						</div>
					</section>

					<section id="manual-device-control" class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-4 scroll-mt-6">
						<div>
							<h2 class="text-xl font-semibold text-slate-900">Manual Device Control</h2>
							<p class="text-sm text-slate-500">Only admins can manually lock and unlock the door.</p>
						</div>

						<article class="rounded-xl border border-slate-100 p-4 flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-between">
							<div>
								<p class="font-semibold text-slate-900">{{ manualDoorControl.name }}</p>
								<p class="text-sm text-slate-500">Current state: {{ manualDoorControl.locked ? 'Locked' : 'Unlocked' }}</p>
							</div>
							<div class="flex items-center gap-3">
								<span class="rounded-full px-3 py-1 text-xs font-semibold" :class="manualDoorControl.locked ? 'bg-amber-100 text-amber-700' : 'bg-emerald-100 text-emerald-700'">
									{{ manualDoorControl.locked ? 'Locked' : 'Unlocked' }}
								</span>
								<button
									type="button"
									class="rounded-lg px-3 py-2 text-sm font-semibold transition-all"
									:class="manualDoorControl.locked ? 'bg-[#00696b] text-white hover:bg-[#004d4f]' : 'bg-rose-100 text-rose-700 hover:bg-rose-200'"
									@click="manualDoorControl.locked = !manualDoorControl.locked"
								>
									{{ manualDoorControl.locked ? 'Unlock Door' : 'Lock Door' }}
								</button>
							</div>
						</article>
						<p class="text-xs text-slate-400">Access to this control is restricted to admin users.</p>
					</section>

					<section id="recent-activity" class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-4 scroll-mt-6">
						<div>
							<h2 class="text-xl font-semibold text-slate-900">Recent Activity</h2>
							<p class="text-sm text-slate-500">Most recent platform events and control actions.</p>
						</div>

						<ul class="space-y-3">
							<li v-for="activity in recentActivity" :key="activity.id" class="rounded-xl border border-slate-100 p-4">
								<div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-2">
									<p class="font-medium text-slate-900">{{ activity.action }}</p>
									<p class="text-xs font-semibold uppercase tracking-wide text-slate-400">{{ activity.time }}</p>
								</div>
								<p class="mt-1 text-sm text-slate-500">{{ activity.detail }}</p>
							</li>
						</ul>
					</section>
				</section>
			</main>
		</div>
	</div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'
import AdminSidebar from '~/components/admin/AdminSidebar.vue'

const deviceList = [
	{ id: 'DV-001', status: 'Online', lastSeenAt: '2 mins ago', unitNumber: '101', propertyName: 'Riverside Suites' },
	{ id: 'DV-002', status: 'Online', lastSeenAt: '5 mins ago', unitNumber: '205', propertyName: 'Riverside Suites' },
	{ id: 'DV-003', status: 'Offline', lastSeenAt: '1 hr ago', unitNumber: '412', propertyName: 'Downtown Plaza' },
	{ id: 'DV-004', status: 'Online', lastSeenAt: '1 min ago', unitNumber: '301', propertyName: 'Green Valley' },
	{ id: 'DV-005', status: 'Online', lastSeenAt: '3 mins ago', unitNumber: '108', propertyName: 'Riverside Suites' },
	{ id: 'DV-006', status: 'Offline', lastSeenAt: '45 mins ago', unitNumber: 'Common', propertyName: 'Downtown Plaza' }
]

const onlineDeviceCount = computed(() => deviceList.filter((d) => d.status === 'Online').length)
const offlineDeviceCount = computed(() => deviceList.filter((d) => d.status === 'Offline').length)

const manualDoorControl = ref({
	name: 'Main Door Access',
	locked: true
})

const recentActivity = [
	{ id: 1, action: 'Generator manually disabled', detail: 'Action executed by Admin at Riverside Block A.', time: '2 min ago' },
	{ id: 2, action: 'Leak sensor offline alert', detail: 'Unit D-12 sensor stopped reporting telemetry.', time: '11 min ago' },
	{ id: 3, action: 'Monthly collection snapshot generated', detail: 'Finance summary exported for April cycle.', time: '28 min ago' },
	{ id: 4, action: 'Gate access policy updated', detail: 'Visitor window adjusted from 6:00 AM to 10:00 PM.', time: '1 hr ago' }
]
</script>

