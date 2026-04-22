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
							<p class="text-sm text-slate-500">Health and operational performance across the full platform.</p>
						</div>

						<div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-4 gap-4">
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Active Properties</p>
								<p class="mt-2 text-2xl font-semibold text-slate-900">56</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Online Devices</p>
								<p class="mt-2 text-2xl font-semibold text-emerald-700">1,240</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Critical Alerts</p>
								<p class="mt-2 text-2xl font-semibold text-rose-700">3</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Automation Success</p>
								<p class="mt-2 text-2xl font-semibold text-[#00696b]">98.2%</p>
							</article>
						</div>
					</section>

					<section id="financial-overview" class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-4 scroll-mt-6">
						<div>
							<h2 class="text-xl font-semibold text-slate-900">Financial Overview</h2>
							<p class="text-sm text-slate-500">Revenue, collections, and outstanding balances for the current cycle.</p>
						</div>

						<div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-4 gap-4">
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Total Billed</p>
								<p class="mt-2 text-2xl font-semibold text-slate-900">Ksh 8.9M</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Collected</p>
								<p class="mt-2 text-2xl font-semibold text-emerald-700">Ksh 8.1M</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Outstanding</p>
								<p class="mt-2 text-2xl font-semibold text-amber-700">Ksh 0.8M</p>
							</article>
							<article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-xs uppercase tracking-wide text-slate-500">Collection Rate</p>
								<p class="mt-2 text-2xl font-semibold text-[#00696b]">91%</p>
							</article>
						</div>
					</section>

					<section id="device-iot-control" class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-4 scroll-mt-6">
						<div>
							<h2 class="text-xl font-semibold text-slate-900">Device/IoT Control</h2>
							<p class="text-sm text-slate-500">Automation profile and live connectivity status per smart device category.</p>
						</div>

						<div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-4">
							<article v-for="device in iotDevices" :key="device.name" class="rounded-xl border border-slate-100 p-4">
								<div class="flex items-center justify-between">
									<p class="font-semibold text-slate-900">{{ device.name }}</p>
									<span class="rounded-full px-2.5 py-1 text-xs font-semibold" :class="device.online ? 'bg-emerald-100 text-emerald-700' : 'bg-rose-100 text-rose-700'">
										{{ device.online ? 'Online' : 'Offline' }}
									</span>
								</div>
								<p class="mt-2 text-sm text-slate-500">Automation mode: {{ device.mode }}</p>
								<p class="mt-1 text-sm text-slate-500">Last sync: {{ device.lastSync }}</p>
							</article>
						</div>
					</section>

					<section id="manual-device-control" class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-4 scroll-mt-6">
						<div>
							<h2 class="text-xl font-semibold text-slate-900">Manual Device Control</h2>
							<p class="text-sm text-slate-500">Override smart routines and trigger direct commands for key endpoints.</p>
						</div>

						<div class="grid grid-cols-1 lg:grid-cols-2 gap-4">
							<article v-for="control in manualControls" :key="control.name" class="rounded-xl border border-slate-100 p-4 flex items-center justify-between gap-4">
								<div>
									<p class="font-semibold text-slate-900">{{ control.name }}</p>
									<p class="text-sm text-slate-500">Current state: {{ control.enabled ? 'Enabled' : 'Disabled' }}</p>
								</div>
								<button
									type="button"
									class="rounded-lg px-3 py-2 text-sm font-semibold transition-all"
									:class="control.enabled ? 'bg-rose-100 text-rose-700 hover:bg-rose-200' : 'bg-[#00696b] text-white hover:bg-[#004d4f]'"
									@click="control.enabled = !control.enabled"
								>
									{{ control.enabled ? 'Disable' : 'Enable' }}
								</button>
							</article>
						</div>
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
import { ref } from 'vue'
import AdminSidebar from '~/components/admin/AdminSidebar.vue'

const iotDevices = [
	{ name: 'Smart Locks', online: true, mode: 'Auto-secure', lastSync: '1 min ago' },
	{ name: 'HVAC Controllers', online: true, mode: 'Energy saver', lastSync: '3 mins ago' },
	{ name: 'Water Leak Sensors', online: false, mode: 'Alert only', lastSync: '27 mins ago' }
]

const manualControls = ref([
	{ name: 'Main Gate Access', enabled: true },
	{ name: 'Parking Lights', enabled: false },
	{ name: 'Boiler Pump', enabled: true },
	{ name: 'Backup Generator', enabled: false }
])

const recentActivity = [
	{ id: 1, action: 'Generator manually disabled', detail: 'Action executed by Admin at Riverside Block A.', time: '2 min ago' },
	{ id: 2, action: 'Leak sensor offline alert', detail: 'Unit D-12 sensor stopped reporting telemetry.', time: '11 min ago' },
	{ id: 3, action: 'Monthly collection snapshot generated', detail: 'Finance summary exported for April cycle.', time: '28 min ago' },
	{ id: 4, action: 'Gate access policy updated', detail: 'Visitor window adjusted from 6:00 AM to 10:00 PM.', time: '1 hr ago' }
]
</script>

