<template>
  <div class="min-h-screen bg-[radial-gradient(circle_at_top_left,_rgba(0,105,107,0.18),_transparent_32%),linear-gradient(135deg,_#dce9e7_0%,_#eef5f4_45%,_#f7fbfb_100%)] text-slate-800">
    <main class="mx-auto flex min-h-screen w-full max-w-7xl items-center px-4 py-8 sm:px-6 lg:px-8">
      <section class="grid w-full gap-8 overflow-hidden rounded-[2rem] border border-white/70 bg-white/85 shadow-[0_24px_80px_rgba(15,23,42,0.12)] backdrop-blur md:grid-cols-[1.1fr_0.9fr]">
        <div class="relative flex flex-col justify-between overflow-hidden bg-[#005f61] px-6 py-8 text-white sm:px-10 sm:py-10">
          <div class="absolute inset-0 bg-[radial-gradient(circle_at_top_right,_rgba(255,255,255,0.18),_transparent_30%),radial-gradient(circle_at_bottom_left,_rgba(255,255,255,0.14),_transparent_28%)]"></div>
          <div class="relative space-y-8">
            <div>
              <p class="text-sm font-semibold uppercase tracking-[0.35em] text-white/70">Serene Habitat</p>
              <h1 class="mt-4 max-w-lg text-4xl font-bold tracking-tight sm:text-5xl">
                Sign in to your space.
              </h1>
              <p class="mt-4 max-w-md text-sm leading-6 text-white/80 sm:text-base">
                Use one login form for tenant, landlord, or admin access. Pick your role, enter your email and password, and continue to the right dashboard.
              </p>
            </div>

            <div class="grid gap-3 sm:grid-cols-3">
              <article v-for="role in roles" :key="role.value" class="rounded-2xl border border-white/15 bg-white/10 p-4 shadow-sm">
                <p class="text-sm font-semibold text-white">{{ role.label }}</p>
                <p class="mt-2 text-xs leading-5 text-white/72">{{ role.description }}</p>
              </article>
            </div>
          </div>

          <div class="relative mt-8 flex items-center justify-between gap-4 rounded-2xl border border-white/15 bg-white/10 px-4 py-4">
            <div>
              <p class="text-xs uppercase tracking-[0.3em] text-white/60">Need a shortcut?</p>
              <p class="mt-1 text-sm text-white/80">Jump directly to a role dashboard.</p>
            </div>
            <div class="flex flex-wrap justify-end gap-2">
              <NuxtLink
                to="/tenant/dashboard"
                class="rounded-full border border-white/20 px-3 py-2 text-xs font-semibold text-white transition hover:bg-white/10"
              >
                Tenant
              </NuxtLink>
              <NuxtLink
                to="/landlord/dashboard"
                class="rounded-full border border-white/20 px-3 py-2 text-xs font-semibold text-white transition hover:bg-white/10"
              >
                Landlord
              </NuxtLink>
              <NuxtLink
                to="/admin/dashboard"
                class="rounded-full border border-white/20 px-3 py-2 text-xs font-semibold text-white transition hover:bg-white/10"
              >
                Admin
              </NuxtLink>
            </div>
          </div>
        </div>

        <div class="flex items-center justify-center px-6 py-8 sm:px-10 sm:py-10">
          <div class="w-full max-w-md space-y-6">
            <div>
              <NuxtLink to="/" class="text-sm font-semibold uppercase tracking-[0.3em] text-[#00696b]">
                Back to Home
              </NuxtLink>
              <h2 class="mt-3 text-3xl font-bold text-slate-900">Log in</h2>
              <p class="mt-2 text-sm leading-6 text-slate-500">
                Enter your email, password, and role to continue.
              </p>
            </div>

            <form class="space-y-5" @submit.prevent="handleLogin">
              <div>
                <label for="role" class="mb-2 block text-sm font-medium text-slate-700">Account type</label>
                <div class="grid gap-2 sm:grid-cols-3">
                  <button
                    v-for="role in roles"
                    :key="role.value"
                    type="button"
                    class="rounded-xl border px-3 py-3 text-left transition-all"
                    :class="selectedRole === role.value ? 'border-[#00696b] bg-[#00696b] text-white shadow-sm' : 'border-slate-200 bg-white text-slate-700 hover:border-slate-300 hover:bg-slate-50'"
                    @click="selectedRole = role.value"
                  >
                    <span class="block text-sm font-semibold">{{ role.label }}</span>
                    <span class="block text-xs opacity-80">{{ role.shortLabel }}</span>
                  </button>
                </div>
              </div>

              <div>
                <label for="email" class="mb-2 block text-sm font-medium text-slate-700">Email address</label>
                <input
                  id="email"
                  v-model.trim="email"
                  type="email"
                  autocomplete="email"
                  placeholder="name@serenehabitat.com"
                  class="w-full rounded-xl border border-slate-300 bg-white px-4 py-3 text-sm text-slate-800 outline-none transition focus:border-[#00696b] focus:ring-4 focus:ring-[#00696b]/10"
                />
              </div>

              <div>
                <label for="password" class="mb-2 block text-sm font-medium text-slate-700">Password</label>
                <input
                  id="password"
                  v-model="password"
                  type="password"
                  autocomplete="current-password"
                  placeholder="Enter your password"
                  class="w-full rounded-xl border border-slate-300 bg-white px-4 py-3 text-sm text-slate-800 outline-none transition focus:border-[#00696b] focus:ring-4 focus:ring-[#00696b]/10"
                />
              </div>

              <div class="flex items-center justify-between gap-3">
                <p v-if="message" class="text-sm" :class="messageType === 'error' ? 'text-rose-600' : 'text-[#00696b]'">
                  {{ message }}
                </p>
                <NuxtLink to="/" class="ml-auto text-sm font-medium text-[#00696b] hover:text-[#004d4f]">
                  Forgot your route?
                </NuxtLink>
              </div>

              <button
                type="submit"
                class="inline-flex w-full items-center justify-center rounded-xl bg-[#00696b] px-4 py-3 text-sm font-semibold text-white shadow-sm transition hover:bg-[#004d4f]"
              >
                Sign in
              </button>
            </form>

            <p class="text-xs leading-5 text-slate-400">
              This starter routes the form to the selected dashboard. Connect it to real authentication later when your backend is ready.
            </p>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'

type Role = 'tenant' | 'landlord' | 'admin'

const roles = [
  { label: 'Tenant', value: 'tenant' as Role, shortLabel: 'Resident access', description: 'Pay rent, review transactions, and manage your profile.' },
  { label: 'Landlord', value: 'landlord' as Role, shortLabel: 'Owner access', description: 'Review units, tickets, and collections in one place.' },
  { label: 'Admin', value: 'admin' as Role, shortLabel: 'Operations', description: 'Track portfolio health and broader operational status.' }
]

const selectedRole = ref<Role>('tenant')
const email = ref('')
const password = ref('')
const message = ref('')
const messageType = ref<'error' | 'success'>('success')

const roleRoutes = computed<Record<Role, string>>(() => ({
  tenant: '/tenant/dashboard',
  landlord: '/landlord/dashboard',
  admin: '/admin/dashboard'
}))

async function handleLogin() {
  message.value = ''

  if (!email.value || !password.value) {
    messageType.value = 'error'
    message.value = 'Enter both your email and password.'
    return
  }

  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email.value)) {
    messageType.value = 'error'
    message.value = 'Enter a valid email address.'
    return
  }

  messageType.value = 'success'
  message.value = `Signing in as ${selectedRole.value}...`
  await navigateTo(roleRoutes.value[selectedRole.value])
}
</script>