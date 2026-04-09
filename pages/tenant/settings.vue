<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <Sidebar />

    <div class="flex-1 flex flex-col">
      <main class="p-4 sm:p-6 flex-1">
        <section class="max-w-5xl mx-auto space-y-6">
          <header class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
            <div>
              <h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Settings</h1>
              <p class="mt-2 text-slate-500 text-sm sm:text-base">
                Manage your personal profile, contact details, and account security.
              </p>
            </div>
            <NuxtLink
              to="/tenant/dashboard"
              class="inline-flex items-center justify-center rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100 transition-colors"
            >
              Back to Dashboard
            </NuxtLink>
          </header>

          <section class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-5">
            <div>
              <h2 class="text-xl font-semibold text-slate-900">Personal Profile & Contact Info</h2>
              <p class="mt-1 text-sm text-slate-500">
                Update your phone number, email address, and profile picture.
              </p>
            </div>

            <div class="flex flex-col sm:flex-row sm:items-center gap-4">
              <img
                :src="profilePreview"
                alt="Tenant profile picture"
                class="h-20 w-20 rounded-full object-cover border border-slate-200"
              />
              <div class="space-y-2">
                <label for="profile-photo" class="text-sm font-medium text-slate-700">Profile photo</label>
                <input
                  id="profile-photo"
                  type="file"
                  accept="image/*"
                  class="block w-full text-sm text-slate-600 file:mr-3 file:px-3 file:py-2 file:rounded-lg file:border-0 file:bg-[#00696b] file:text-white hover:file:bg-[#004d4f]"
                  @change="onPhotoSelected"
                />
              </div>
            </div>

            <form class="grid grid-cols-1 sm:grid-cols-2 gap-4" @submit.prevent="saveContactInfo">
              <div class="space-y-2">
                <label for="full-name" class="text-sm font-medium text-slate-700">Full name</label>
                <input
                  id="full-name"
                  v-model="fullName"
                  type="text"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                />
              </div>

              <div class="space-y-2">
                <label for="phone" class="text-sm font-medium text-slate-700">Phone number</label>
                <input
                  id="phone"
                  v-model="phone"
                  type="tel"
                  placeholder="e.g. +254 712 345 678"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                />
              </div>

              <div class="space-y-2 sm:col-span-2">
                <label for="email" class="text-sm font-medium text-slate-700">Email address</label>
                <input
                  id="email"
                  v-model="email"
                  type="email"
                  placeholder="e.g. tenant@serenehabitat.com"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                />
              </div>

              <div class="sm:col-span-2 flex items-center justify-between gap-3 pt-1">
                <p v-if="saveMessage" class="text-sm text-green-700">{{ saveMessage }}</p>
                <button
                  type="submit"
                  class="ml-auto inline-flex items-center justify-center rounded-lg bg-[#00696b] px-4 py-2 text-sm font-semibold text-white hover:bg-[#004d4f] transition-colors"
                >
                  Save Profile Updates
                </button>
              </div>
            </form>
          </section>

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

          <section id="support-request" class="bg-white rounded-2xl border border-slate-100 shadow-sm p-5 sm:p-6 space-y-5">
            <div>
              <h2 class="text-xl font-semibold text-slate-900">Support Request</h2>
              <p class="mt-1 text-sm text-slate-500">
                Share your contact details and what support you need.
              </p>
            </div>

            <form class="grid grid-cols-1 sm:grid-cols-2 gap-4" @submit.prevent="submitSupportRequest">
              <div class="space-y-2">
                <label for="support-email" class="text-sm font-medium text-slate-700">Email address</label>
                <input
                  id="support-email"
                  v-model="supportEmail"
                  type="email"
                  placeholder="e.g. tenant@serenehabitat.com"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                />
              </div>

              <div class="space-y-2">
                <label for="support-contact" class="text-sm font-medium text-slate-700">Contact number</label>
                <input
                  id="support-contact"
                  v-model="supportContact"
                  type="tel"
                  placeholder="e.g. +254 712 345 678"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                />
              </div>

              <div class="space-y-2 sm:col-span-2">
                <label for="support-message" class="text-sm font-medium text-slate-700">Request details</label>
                <textarea
                  id="support-message"
                  v-model="supportRequest"
                  rows="4"
                  placeholder="Describe what you need help with"
                  class="w-full rounded-lg border border-slate-300 px-3 py-2 text-sm focus:outline-none focus:ring-2 focus:ring-[#00696b]"
                />
              </div>

              <div class="sm:col-span-2 flex items-center justify-between gap-3 pt-1">
                <p v-if="supportMessage" class="text-sm text-[#00696b]">{{ supportMessage }}</p>
                <button
                  type="submit"
                  class="ml-auto inline-flex items-center justify-center rounded-lg bg-[#00696b] px-4 py-2 text-sm font-semibold text-white hover:bg-[#004d4f] transition-colors"
                >
                  Submit Request
                </button>
              </div>
            </form>
          </section>
        </section>
      </main>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import Sidebar from '~/components/shared/SharedSidebar.vue'

const fullName = ref('Ashley Tenant')
const phone = ref('+254 712 345 678')
const email = ref('ashley.tenant@serenehabitat.com')
const profilePreview = ref('https://images.unsplash.com/photo-1494790108377-be9c29b29330?auto=format&fit=crop&w=200&q=80')

const saveMessage = ref('')
const securityMessage = ref('')
const currentPassword = ref('')
const newPassword = ref('')
const confirmPassword = ref('')
const supportEmail = ref('')
const supportContact = ref('')
const supportRequest = ref('')
const supportMessage = ref('')

function onPhotoSelected(event: Event) {
  const target = event.target as HTMLInputElement
  const file = target.files?.[0]
  if (!file) return

  profilePreview.value = URL.createObjectURL(file)
  saveMessage.value = 'Profile photo selected. Click Save Profile Updates to confirm.'
}

function saveContactInfo() {
  saveMessage.value = 'Your profile and contact details were updated successfully.'
}

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

function submitSupportRequest() {
  if (!supportEmail.value || !supportContact.value || !supportRequest.value.trim()) {
    supportMessage.value = 'Please fill in email, contact number, and request details.'
    return
  }

  supportMessage.value = 'Support request submitted. Our team will contact you shortly.'
  supportRequest.value = ''
}
</script>
