<template>
	<div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
		<AdminSidebar />

		<div class="flex-1 flex flex-col">
			<main class="p-4 sm:p-6 flex-1">
				<section class="max-w-5xl mx-auto space-y-6">
					<header>
						<h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Manual Device Control</h1>
						<p class="mt-2 text-slate-500">Only admins can manually lock and unlock the door.</p>
					</header>

					<section class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-4">
						<div class="flex flex-col gap-4 sm:flex-row sm:items-center sm:justify-between">
							<div>
								<h2 class="text-xl font-semibold text-slate-900">Main Door Access</h2>
								<p class="text-sm text-slate-500">Current state: Locked</p>
							</div>

							<div class="flex items-center gap-3">
								<span class="rounded-full bg-amber-100 px-3 py-1 text-xs font-semibold text-amber-700">Locked</span>
								<button
									type="button"
									class="rounded-lg bg-[#00696b] px-4 py-2.5 text-sm font-semibold text-white transition hover:bg-[#004d4f]"
								>
									Unlock Door
								</button>
							</div>
						</div>
						<p class="text-xs text-slate-400">Access to this control is restricted to admin users.</p>
					</section>
				</section>
			</main>
		</div>
	</div>
</template>

<script setup lang="ts">
import AdminSidebar from '~/components/admin/AdminSidebar.vue'
</script>
