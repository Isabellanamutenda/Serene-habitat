<template>
  <aside
    class="w-full lg:w-64 shrink-0 bg-white border-b border-slate-100 lg:border-b-0 lg:border-r p-4 lg:p-6"
  >
    <div class="flex items-center justify-between">
      <h2 class="text-[#00696b] font-bold text-lg sm:text-xl">Landlord Panel</h2>
      <button
        type="button"
        @click="isMenuCollapsed = !isMenuCollapsed"
        :aria-label="isMenuCollapsed ? 'Expand menu' : 'Collapse menu'"
        class="h-9 w-9 rounded-lg border border-slate-200 text-slate-600 hover:text-[#00696b] hover:border-[#00696b] transition-all"
      >
        {{ isMenuCollapsed ? 'v' : '^' }}
      </button>
    </div>

    <div
      class="overflow-hidden transition-all duration-300"
      :class="isMenuCollapsed ? 'max-h-0 opacity-0 mt-0' : 'max-h-[36rem] opacity-100 mt-4'"
    >
      <nav class="flex flex-col space-y-2">
        <NuxtLink
          to="/landlord/dashboard#overview"
          class="flex items-center space-x-3 text-slate-600 hover:text-[#00696b] font-medium p-2 rounded-lg transition-all"
        >
          <span>📊</span>
          <span>Overview</span>
        </NuxtLink>

        <NuxtLink
          to="/landlord/dashboard#units"
          class="flex items-center space-x-3 text-slate-600 hover:text-[#00696b] font-medium p-2 rounded-lg transition-all"
        >
          <span>🏢</span>
          <span>Units</span>
        </NuxtLink>

        <NuxtLink
          to="/landlord/dashboard#tenants"
          class="flex items-center space-x-3 text-slate-600 hover:text-[#00696b] font-medium p-2 rounded-lg transition-all"
        >
          <span>👥</span>
          <span>Tenants</span>
        </NuxtLink>

        <NuxtLink
          to="/landlord/dashboard#maintenance"
          class="flex items-center space-x-3 text-slate-600 hover:text-[#00696b] font-medium p-2 rounded-lg transition-all"
        >
          <span>🛠️</span>
          <span>Maintenance</span>
        </NuxtLink>

        <NuxtLink
          to="/landlord/dashboard#financials"
          class="flex items-center space-x-3 text-slate-600 hover:text-[#00696b] font-medium p-2 rounded-lg transition-all"
        >
          <span>💰</span>
          <span>Financials</span>
        </NuxtLink>

        <NuxtLink
          to="/landlord/settings"
          class="flex items-center space-x-3 text-slate-600 hover:text-[#00696b] font-medium p-2 rounded-lg transition-all"
        >
          <span>⚙️</span>
          <span>Settings</span>
        </NuxtLink>
      </nav>
    </div>
  </aside>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const isMenuCollapsed = ref(false)
</script>