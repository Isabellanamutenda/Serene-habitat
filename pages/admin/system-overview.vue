<template>
	<div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
		<AdminSidebar />

		<div class="flex-1 flex flex-col">
			<main class="p-4 sm:p-6 flex-1">
				<section class="max-w-7xl mx-auto space-y-6">
					<header>
						<h1 class="text-3xl sm:text-4xl font-bold text-slate-900">System Overview</h1>
						<p class="mt-2 text-slate-500">Platform-wide entities and resource allocation summary.</p>
					</header>

					<div class="grid grid-cols-1 sm:grid-cols-2 xl:grid-cols-4 gap-4">
						<article class="rounded-2xl border border-slate-100 bg-white shadow-sm p-5">
							<p class="text-xs uppercase tracking-wide text-slate-500">Total Organizations</p>
							<p class="mt-2 text-3xl font-semibold text-slate-900">8</p>
						</article>
						<article class="rounded-2xl border border-slate-100 bg-white shadow-sm p-5">
							<p class="text-xs uppercase tracking-wide text-slate-500">Total Landlords</p>
							<p class="mt-2 text-3xl font-semibold text-slate-900">42</p>
						</article>
						<article class="rounded-2xl border border-slate-100 bg-white shadow-sm p-5">
							<p class="text-xs uppercase tracking-wide text-slate-500">Total Units</p>
							<p class="mt-2 text-3xl font-semibold text-emerald-700">487</p>
						</article>
						<article class="rounded-2xl border border-slate-100 bg-white shadow-sm p-5">
							<p class="text-xs uppercase tracking-wide text-slate-500">Total Tenants</p>
							<p class="mt-2 text-3xl font-semibold text-[#00696b]">478</p>
						</article>
					</div>

					<section class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-4">
						<div>
							<h2 class="text-xl font-semibold text-slate-900">Platform Snapshot</h2>
							<p class="text-sm text-slate-500">A quick read on the current portfolio mix and occupancy scale.</p>
						</div>

						<div class="grid grid-cols-1 md:grid-cols-3 gap-4">
							<div class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-sm text-slate-500">Properties</p>
								<p class="mt-2 text-2xl font-semibold text-slate-900">56</p>
							</div>
							<div class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-sm text-slate-500">Online Devices</p>
								<p class="mt-2 text-2xl font-semibold text-emerald-700">4,218</p>
							</div>
							<div class="rounded-xl border border-slate-100 bg-slate-50 p-4">
								<p class="text-sm text-slate-500">Offline Devices</p>
								<p class="mt-2 text-2xl font-semibold text-rose-700">12</p>
							</div>
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
