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
							<p class="text-sm text-slate-500">Latest payments and device command actions across the platform.</p>
						</div>

						<div class="grid grid-cols-1 xl:grid-cols-2 gap-6">
							<section class="rounded-xl border border-slate-100 p-4">
								<header class="mb-3">
									<h3 class="font-semibold text-slate-900">Latest Payments (10)</h3>
								</header>
								<div class="overflow-x-auto">
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
											<tr v-for="payment in latestPayments" :key="payment.ref" class="border-b border-slate-50">
												<td class="py-2 pr-4 font-semibold text-slate-900">{{ payment.ref }}</td>
												<td class="py-2 pr-4 text-slate-700">{{ payment.tenant }}</td>
												<td class="py-2 pr-4 text-emerald-700 font-semibold">{{ payment.amount }}</td>
												<td class="py-2 text-slate-500">{{ payment.time }}</td>
											</tr>
										</tbody>
									</table>
								</div>
							</section>

							<section class="rounded-xl border border-slate-100 p-4">
								<header class="mb-3">
									<h3 class="font-semibold text-slate-900">Latest Device Commands (10)</h3>
								</header>
								<div class="overflow-x-auto">
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
											<tr v-for="command in latestDeviceCommands" :key="command.id" class="border-b border-slate-50">
												<td class="py-2 pr-4 font-semibold text-slate-900">{{ command.command }}</td>
												<td class="py-2 pr-4 text-slate-700">{{ command.deviceId }}</td>
												<td class="py-2 pr-4 text-slate-700">{{ command.admin }}</td>
												<td class="py-2 text-slate-500">{{ command.time }}</td>
											</tr>
										</tbody>
									</table>
								</div>
							</section>
						</div>
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

const latestPayments = [
	{ ref: 'MPX00172', tenant: 'A. Mugo', amount: 'Ksh 45,000', time: '2 min ago' },
	{ ref: 'MPX00171', tenant: 'N. Wanjiku', amount: 'Ksh 38,500', time: '6 min ago' },
	{ ref: 'MPX00170', tenant: 'K. Otieno', amount: 'Ksh 41,250', time: '11 min ago' },
	{ ref: 'MPX00169', tenant: 'L. Kilonzo', amount: 'Ksh 52,000', time: '18 min ago' },
	{ ref: 'MPX00168', tenant: 'D. Kamau', amount: 'Ksh 36,000', time: '25 min ago' },
	{ ref: 'MPX00167', tenant: 'R. Achieng', amount: 'Ksh 47,000', time: '31 min ago' },
	{ ref: 'MPX00166', tenant: 'M. Wekesa', amount: 'Ksh 33,000', time: '42 min ago' },
	{ ref: 'MPX00165', tenant: 'T. Njeri', amount: 'Ksh 40,500', time: '53 min ago' },
	{ ref: 'MPX00164', tenant: 'P. Mwangi', amount: 'Ksh 39,000', time: '1 hr ago' },
	{ ref: 'MPX00163', tenant: 'B. Kiptoo', amount: 'Ksh 43,000', time: '1 hr ago' }
]

const latestDeviceCommands = [
	{ id: 1, command: 'Unlock Door', deviceId: 'DV-001', admin: 'Admin User', time: '1 min ago' },
	{ id: 2, command: 'Lock Door', deviceId: 'DV-001', admin: 'Admin User', time: '4 min ago' },
	{ id: 3, command: 'Restart Sensor', deviceId: 'DV-006', admin: 'Admin User', time: '9 min ago' },
	{ id: 4, command: 'Unlock Door', deviceId: 'DV-004', admin: 'Admin User', time: '14 min ago' },
	{ id: 5, command: 'Lock Door', deviceId: 'DV-004', admin: 'Admin User', time: '19 min ago' },
	{ id: 6, command: 'Ping Device', deviceId: 'DV-003', admin: 'Admin User', time: '27 min ago' },
	{ id: 7, command: 'Unlock Door', deviceId: 'DV-002', admin: 'Admin User', time: '36 min ago' },
	{ id: 8, command: 'Lock Door', deviceId: 'DV-002', admin: 'Admin User', time: '43 min ago' },
	{ id: 9, command: 'Restart Controller', deviceId: 'DV-005', admin: 'Admin User', time: '51 min ago' },
	{ id: 10, command: 'Sync Device', deviceId: 'DV-003', admin: 'Admin User', time: '1 hr ago' }
]
</script>

