<template>
	<div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
		<Sidebar />

		<div class="flex-1 flex flex-col">
			<main class="p-4 sm:p-6 flex-1">
				<section class="max-w-6xl mx-auto space-y-6">
					<header>
						<h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Admin Dashboard</h1>
						<p class="mt-2 text-slate-500">Track occupancy, billing performance, and operational health at a glance.</p>
					</header>

					<div id="transactions" class="grid grid-cols-1 md:grid-cols-2 gap-4">
						<article class="card">
							<p class="text-sm text-slate-500">Occupancy Rate</p>
							<p class="text-3xl font-semibold text-slate-800 mt-2">92%</p>
						</article>

						<article class="card">
							<p class="text-sm text-slate-500">Monthly Collections</p>
							<p class="text-3xl font-semibold text-slate-800 mt-2">Ksh 2.4M</p>
						</article>
					</div>

					<section id="settings" class="card">
						<p class="text-sm text-slate-500">System Health</p>
						<p class="text-3xl font-semibold text-slate-800 mt-2">All Services Operational</p>
					</section>
				</section>
			</main>
		</div>
	</div>
</template>

<script setup lang="ts">
import Sidebar from '~/components/shared/SharedSidebar.vue'
</script>

