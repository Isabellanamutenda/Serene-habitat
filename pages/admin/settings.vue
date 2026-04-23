<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <AdminSidebar />

    <div class="flex-1 flex flex-col">
      <main class="p-4 sm:p-6 flex-1">
        <section class="max-w-3xl mx-auto space-y-6">
          <header>
            <h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Admin Settings</h1>
            <p class="mt-2 text-slate-500">Manage your account session and access controls.</p>
          </header>

          <section class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-4">
            <div>
              <h2 class="text-xl font-semibold text-slate-900">Session Status</h2>
              <p class="text-sm text-slate-500">Use these controls to securely sign in or sign out.</p>
            </div>

            <div
              class="rounded-xl border p-4"
              :class="isLoggedIn ? 'border-emerald-200 bg-emerald-50/60' : 'border-amber-200 bg-amber-50/70'"
            >
              <p class="text-sm font-semibold" :class="isLoggedIn ? 'text-emerald-700' : 'text-amber-700'">
                {{ isLoggedIn ? 'Logged in' : 'Logged out' }}
              </p>
              <p class="mt-2 text-sm text-slate-600">
                {{ sessionSummary }}
              </p>
            </div>

            <div class="flex flex-wrap gap-3">
              <button
                type="button"
                class="rounded-lg bg-[#00696b] px-4 py-2.5 text-sm font-semibold text-white transition hover:bg-[#004d4f]"
                @click="handleLogin"
              >
                {{ isLoggedIn ? 'Switch Account' : 'Log in' }}
              </button>

              <button
                type="button"
                class="rounded-lg border border-rose-200 bg-rose-50 px-4 py-2.5 text-sm font-semibold text-rose-700 transition hover:bg-rose-100 disabled:cursor-not-allowed disabled:opacity-50"
                :disabled="!isLoggedIn"
                @click="handleLogout"
              >
                Log out
              </button>
            </div>
          </section>
        </section>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed, onMounted, ref } from 'vue'
import AdminSidebar from '~/components/admin/AdminSidebar.vue'

type Role = 'tenant' | 'landlord' | 'admin'
type AuthSession = {
  role: Role
  email: string
  loggedInAt: string
}

const AUTH_STORAGE_KEY = 'serene-auth-session'
const session = ref<AuthSession | null>(null)

const isLoggedIn = computed(() => Boolean(session.value?.email))

const sessionSummary = computed(() => {
  if (!session.value) {
    return 'No active session found. Sign in to continue with admin access.'
  }

  const formattedDate = new Date(session.value.loggedInAt).toLocaleString()
  return `Signed in as ${session.value.email} (${session.value.role}) since ${formattedDate}.`
})

function loadSession() {
  if (!import.meta.client) return

  const rawSession = localStorage.getItem(AUTH_STORAGE_KEY)
  if (!rawSession) {
    session.value = null
    return
  }

  try {
    const parsed = JSON.parse(rawSession) as AuthSession
    session.value = parsed
  } catch {
    session.value = null
    localStorage.removeItem(AUTH_STORAGE_KEY)
  }
}

async function handleLogin() {
  await navigateTo('/login')
}

async function handleLogout() {
  if (import.meta.client) {
    localStorage.removeItem(AUTH_STORAGE_KEY)
  }
  session.value = null
  await navigateTo('/login')
}

onMounted(() => {
  loadSession()
})
</script>