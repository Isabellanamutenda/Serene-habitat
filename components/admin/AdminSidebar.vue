<template>
  <aside
    class="w-full lg:w-72 shrink-0 bg-white border-b border-slate-100 lg:border-b-0 lg:border-r p-4 lg:p-6"
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
      :class="isMenuCollapsed ? 'max-h-0 opacity-0 mt-0' : 'max-h-[44rem] opacity-100 mt-4'"
    >
      <p class="text-xs font-semibold uppercase tracking-wide text-slate-400">Navigation</p>

      <nav class="mt-3 flex flex-col space-y-2">
        <NuxtLink
          v-for="item in items"
          :key="item.label"
          :to="item.to"
          class="flex items-center gap-3 rounded-lg p-2 font-medium transition-all"
          :class="isItemActive(item) ? 'bg-[#e6f4f1] text-[#00696b]' : 'text-slate-600 hover:text-[#00696b] hover:bg-slate-50'"
        >
          <span>{{ item.icon }}</span>
          <span>{{ item.label }}</span>
        </NuxtLink>
      </nav>

      <div class="mt-5 rounded-xl border border-[#cde7e2] bg-[#f2fbf9] p-4">
        <p class="text-xs uppercase tracking-wide text-[#00696b] font-semibold">Admin tip</p>
        <p class="mt-2 text-sm text-slate-600">Use manual controls only when an IoT automation rule fails to trigger.</p>
      </div>
    </div>
  </aside>
</template>

<script setup lang="ts">
import { onBeforeUnmount, onMounted, ref } from 'vue'
import { useRoute } from 'vue-router'

const isMenuCollapsed = ref(false)
const activeSectionId = ref('system-overview')
const route = useRoute()

let sectionObserver: IntersectionObserver | null = null

const items = [
  { id: 'system-overview', label: 'System Overview', to: '/admin/dashboard#system-overview', icon: '📊' },
  { id: 'financial-overview', label: 'Financial Overview', to: '/admin/dashboard#financial-overview', icon: '💳' },
  { id: 'device-iot-control', label: 'Device/IoT Control', to: '/admin/dashboard#device-iot-control', icon: '📡' },
  { id: 'manual-device-control', label: 'Manual Device Control', to: '/admin/dashboard#manual-device-control', icon: '🕹️' },
  { id: 'recent-activity', label: 'Recent Activity', to: '/admin/dashboard#recent-activity', icon: '🧾' },
  { id: 'admin-settings', label: 'Settings', to: '/admin/settings', icon: '⚙️' }
]

function isItemActive(item: { id: string; to: string }) {
  if (item.to === '/admin/settings') {
    return route.path === '/admin/settings'
  }

  return route.path === '/admin/dashboard' && activeSectionId.value === item.id
}

onMounted(() => {
  const sectionIds = items.map((item) => item.id)

  const hashId = window.location.hash.replace('#', '')
  if (hashId && sectionIds.includes(hashId)) {
    activeSectionId.value = hashId
  }

  sectionObserver = new IntersectionObserver(
    (entries) => {
      const visibleEntries = entries
        .filter((entry) => entry.isIntersecting)
        .sort((a, b) => b.intersectionRatio - a.intersectionRatio)

      const firstVisibleEntry = visibleEntries[0]
      if (firstVisibleEntry) {
        activeSectionId.value = firstVisibleEntry.target.id
      }
    },
    {
      root: null,
      rootMargin: '-20% 0px -55% 0px',
      threshold: [0.2, 0.35, 0.5, 0.75]
    }
  )

  sectionIds.forEach((id) => {
    const sectionEl = document.getElementById(id)
    if (sectionEl) sectionObserver?.observe(sectionEl)
  })

  const onHashChange = () => {
    const nextHash = window.location.hash.replace('#', '')
    if (nextHash && sectionIds.includes(nextHash)) {
      activeSectionId.value = nextHash
    }
  }

  window.addEventListener('hashchange', onHashChange)

  onBeforeUnmount(() => {
    window.removeEventListener('hashchange', onHashChange)
  })
})

onBeforeUnmount(() => {
  if (sectionObserver) {
    sectionObserver.disconnect()
    sectionObserver = null
  }
})
</script>

