<template>
  <aside
    class="w-full lg:w-64 shrink-0 bg-white border-b border-slate-100 lg:border-b-0 lg:border-r p-4 lg:p-6"
  >
    <div class="flex items-center justify-between">
      <h2 class="text-[#00696b] font-bold text-lg sm:text-xl">The Serene Habitat</h2>
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
      :class="isMenuCollapsed ? 'max-h-0 opacity-0 mt-0' : 'max-h-96 opacity-100 mt-4'"
    >
      <nav class="flex flex-col space-y-2">
        <NuxtLink
          :to="dashboardPath"
          class="flex items-center space-x-3 text-slate-600 hover:text-[#00696b] font-medium p-2 rounded-lg transition-all"
        >
          <span>🏠</span>
          <span>Home</span>
        </NuxtLink>

        <NuxtLink
          :to="`${dashboardPath}#transactions`"
          class="flex items-center space-x-3 text-slate-600 hover:text-[#00696b] font-medium p-2 rounded-lg transition-all"
        >
          <span>💳</span>
          <span>Transactions</span>
        </NuxtLink>

        <NuxtLink
          :to="settingsPath"
          class="flex items-center space-x-3 text-slate-600 hover:text-[#00696b] font-medium p-2 rounded-lg transition-all"
        >
          <span>⚙️</span>
          <span>Settings</span>
        </NuxtLink>
      </nav>

      <button
        type="button"
        class="mt-4 w-full bg-[#00696b] text-white rounded-2xl font-semibold transition-all p-3"
      >
        Support Request
      </button>
    </div>
  </aside>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'

const isMenuCollapsed = ref(false)
const route = useRoute()

const dashboardPath = computed(() => {
  if (route.path.startsWith('/admin')) return '/admin/dashboard'
  if (route.path.startsWith('/landlord')) return '/landlord/dashboard'
  return '/tenant/dashboard'
})

const settingsPath = computed(() => {
  if (route.path.startsWith('/tenant')) return '/tenant/settings'
  return `${dashboardPath.value}#settings`
})
</script>