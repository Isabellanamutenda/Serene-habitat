<template>
  <aside
    class="w-full lg:w-64 shrink-0 bg-white border-b border-slate-100 lg:border-b-0 lg:border-r p-4 lg:p-6"
  >
    <div class="flex items-center justify-between">
      <h2 class="text-[#00696b] font-bold text-lg sm:text-xl">Rental Agency</h2>
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
          v-for="item in sidebarItems"
          :key="item.label"
          :to="item.to"
          class="flex items-center space-x-3 text-slate-600 hover:text-[#00696b] font-medium p-2 rounded-lg transition-all"
        >
          <span>{{ item.icon }}</span>
          <span>{{ item.label }}</span>
        </NuxtLink>
      </nav>

      <NuxtLink
        :to="supportPath"
        class="mt-4 w-full inline-flex items-center justify-center bg-[#00696b] text-white rounded-2xl font-semibold transition-all p-3"
      >
        Support Request
      </NuxtLink>
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
  if (route.path.startsWith('/landlord')) return '/landlord/settings'
  return `${dashboardPath.value}#settings`
})

const supportPath = computed(() => {
  if (route.path.startsWith('/tenant')) return '/tenant/settings#support-request'
  if (route.path.startsWith('/landlord')) return '/landlord/settings#support-request'
  return `${dashboardPath.value}#support-request`
})

const sidebarItems = computed(() => {
  if (route.path.startsWith('/landlord')) {
    return [
      { label: 'Units', to: '/landlord/dashboard#units', icon: '🏢' },
      { label: 'Tenants', to: '/landlord/dashboard#tenants', icon: '🧑‍🤝‍🧑' },
      { label: 'Maintenance', to: '/landlord/dashboard#maintenance', icon: '🛠️' },
      { label: 'Financials', to: '/landlord/dashboard#financials', icon: '💳' },
      { label: 'Settings', to: settingsPath.value, icon: '⚙️' }
    ]
  }

  return [
    { label: 'Home', to: dashboardPath.value, icon: '🏠' },
    { label: 'Transactions', to: `${dashboardPath.value}#transactions`, icon: '💳' },
    { label: 'Settings', to: settingsPath.value, icon: '⚙️' }
  ]
})
</script>