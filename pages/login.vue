<template>
  <div class="min-h-screen bg-[linear-gradient(180deg,_#eef5f4_0%,_#dce9e7_100%)] text-slate-800">
    <main class="mx-auto flex min-h-screen w-full max-w-2xl items-center px-4 py-8 sm:px-6">
      <section class="w-full rounded-3xl border border-white/70 bg-white p-6 shadow-[0_20px_60px_rgba(15,23,42,0.1)] sm:p-8">
        <div class="mb-6 text-center">
          <p class="text-sm font-semibold uppercase tracking-[0.3em] text-[#00696b]">Serene Habitat</p>
          <h1 class="mt-2 text-3xl font-bold text-slate-900">Log in</h1>
          <p class="mt-2 text-sm text-slate-500">Use your email, password, and role to continue.</p>
        </div>

        <form class="space-y-4" @submit.prevent="handleLogin">
          <div>
            <label for="role" class="mb-2 block text-sm font-medium text-slate-700">Role</label>
            <select
              id="role"
              v-model="selectedRole"
              class="w-full rounded-xl border border-slate-300 bg-white px-4 py-3 text-sm text-slate-800 outline-none transition focus:border-[#00696b] focus:ring-4 focus:ring-[#00696b]/10"
            >
              <option v-for="role in roles" :key="role.value" :value="role.value">
                {{ role.label }}
              </option>
            </select>
          </div>

          <div>
            <label for="email" class="mb-2 block text-sm font-medium text-slate-700">Email</label>
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

          <p v-if="message" class="text-sm" :class="messageType === 'error' ? 'text-rose-600' : 'text-[#00696b]'">
            {{ message }}
          </p>

          <button
            type="submit"
            class="inline-flex w-full items-center justify-center rounded-xl bg-[#00696b] px-4 py-3 text-sm font-semibold text-white shadow-sm transition hover:bg-[#004d4f]"
          >
            Sign in
          </button>
        </form>

        <div class="mt-5 flex flex-wrap justify-center gap-2 text-sm text-slate-500">
          <NuxtLink to="/tenant/dashboard" class="rounded-full border border-slate-200 px-3 py-1.5 hover:bg-slate-50">Tenant</NuxtLink>
          <NuxtLink to="/landlord/dashboard" class="rounded-full border border-slate-200 px-3 py-1.5 hover:bg-slate-50">Landlord</NuxtLink>
          <NuxtLink to="/admin/dashboard" class="rounded-full border border-slate-200 px-3 py-1.5 hover:bg-slate-50">Admin</NuxtLink>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'

type Role = 'tenant' | 'landlord' | 'admin'

const roles = [
  { label: 'Tenant', value: 'tenant' as Role },
  { label: 'Landlord', value: 'landlord' as Role },
  { label: 'Admin', value: 'admin' as Role }
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