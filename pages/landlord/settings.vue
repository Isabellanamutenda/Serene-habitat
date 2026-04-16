<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <LandlordSidebar />

    <div class="flex-1 flex flex-col">
      <main class="p-4 sm:p-6 flex-1">
        <section class="max-w-6xl mx-auto space-y-6">
          <header class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
            <div>
              <h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Settings</h1>
              <p class="mt-2 text-slate-500 text-sm sm:text-base">
                Manage your account access and password security.
              </p>
            </div>
            <NuxtLink
              to="/landlord/dashboard"
              class="inline-flex items-center justify-center rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100 transition-colors"
            >
              Back to Dashboard
            </NuxtLink>
          </header>

          <section class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-5">
            <div>
              <h2 class="text-xl font-semibold text-slate-900">Privacy & Security</h2>
              <p class="mt-1 text-sm text-slate-500">
                Control your account access and password.
              </p>
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
              <button
                type="button"
                class="inline-flex items-center justify-center rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100 transition-colors"
                @click="onLogin"
              >
                Log In
              </button>

              <button
                type="button"
                class="inline-flex items-center justify-center rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100 transition-colors"
                @click="onLogout"
              >
                Log Out
              </button>
            </div>

            <form class="max-w-lg grid grid-cols-1 gap-4" @submit.prevent="onChangePassword">
              <div class="space-y-2">
                <h3 class="text-base font-semibold text-slate-900">Change Password</h3>
              </div>

              <div class="space-y-2">
                <label for="current-password" class="text-sm font-medium text-slate-700">Current password</label>
                <input
                  id="current-password"
                  v-model="currentPassword"
                  type="password"
                  autocomplete="current-password"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                />
              </div>

              <div class="space-y-2">
                <label for="new-password" class="text-sm font-medium text-slate-700">New password</label>
                <input
                  id="new-password"
                  v-model="newPassword"
                  type="password"
                  autocomplete="new-password"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                />
                <p class="text-xs text-slate-500">
                  Must be at least 8 characters and include a letter, number, and special character.
                </p>
              </div>

              <div class="space-y-2">
                <label for="confirm-password" class="text-sm font-medium text-slate-700">Confirm password</label>
                <input
                  id="confirm-password"
                  v-model="confirmPassword"
                  type="password"
                  autocomplete="new-password"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                />
              </div>

              <div class="flex justify-end">
                <button
                  type="submit"
                  class="inline-flex items-center justify-center rounded-lg bg-slate-900 px-4 py-2 text-sm font-semibold text-white hover:bg-slate-700 transition-colors"
                >
                  Update Password
                </button>
              </div>
            </form>

            <p v-if="securityMessage" class="text-sm text-[#00696b]">{{ securityMessage }}</p>
          </section>
        </section>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import LandlordSidebar from '~/components/landlord/LandlordSidebar.vue'

const securityMessage = ref('')
const currentPassword = ref('')
const newPassword = ref('')
const confirmPassword = ref('')

function onLogin() {
  navigateTo('/login')
}

function onLogout() {
  navigateTo('/login')
}

function onChangePassword() {
  if (!currentPassword.value || !newPassword.value || !confirmPassword.value) {
    securityMessage.value = 'Please fill in all password fields.'
    return
  }

  const strongPasswordPattern = /^(?=.*[A-Za-z])(?=.*\d)(?=.*[^A-Za-z\d]).{8,}$/
  if (!strongPasswordPattern.test(newPassword.value)) {
    securityMessage.value = 'New password must be at least 8 characters and include a letter, number, and special character.'
    return
  }

  if (newPassword.value !== confirmPassword.value) {
    securityMessage.value = 'New password and confirm password do not match.'
    return
  }

  securityMessage.value = 'Password updated successfully.'
  currentPassword.value = ''
  newPassword.value = ''
  confirmPassword.value = ''
}
</script>
