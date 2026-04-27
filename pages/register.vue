<template>
  <div class="min-h-screen bg-[radial-gradient(circle_at_top_right,_rgba(0,105,107,0.18),_transparent_34%),linear-gradient(180deg,_#f2f7f6_0%,_#dce9e7_100%)] text-slate-800">
    <main class="mx-auto flex min-h-screen w-full max-w-6xl items-center px-4 py-10 sm:px-6 lg:px-8">
      <section class="grid w-full overflow-hidden rounded-[2rem] border border-white/70 bg-white/80 shadow-[0_20px_60px_rgba(15,23,42,0.12)] backdrop-blur lg:grid-cols-[0.95fr_1.05fr]">
        <div class="relative hidden overflow-hidden bg-[#00696b] p-10 text-white lg:flex lg:flex-col lg:justify-between">
          <div class="absolute -right-12 top-0 h-48 w-48 rounded-full bg-white/10 blur-3xl"></div>
          <div class="absolute -bottom-10 left-0 h-40 w-40 rounded-full bg-white/10 blur-3xl"></div>

          <div class="relative z-10 space-y-6">
            <p class="text-sm font-semibold uppercase tracking-[0.35em] text-white/75">Rental Agency</p>
            <h1 class="max-w-md text-4xl font-bold tracking-tight sm:text-5xl">Create your account in a few steps.</h1>
            <p class="max-w-md text-base leading-7 text-white/80">
              Register once, then use your profile to continue as a tenant, landlord, or admin.
            </p>
          </div>

          <div class="relative z-10 grid gap-3 text-sm text-white/85">
            <div class="rounded-2xl border border-white/15 bg-white/10 p-4 backdrop-blur-sm">Fast setup for new residents and team members.</div>
            <div class="rounded-2xl border border-white/15 bg-white/10 p-4 backdrop-blur-sm">Clear consent at the bottom before you submit.</div>
          </div>
        </div>

        <div class="p-6 sm:p-8 lg:p-10">
          <div class="mb-6 text-center lg:text-left">
            <p class="text-sm font-semibold uppercase tracking-[0.3em] text-[#00696b]">Join Rental Agency</p>
            <h2 class="mt-2 text-3xl font-bold tracking-tight text-slate-900">Register</h2>
            <p class="mt-2 text-sm text-slate-500">Enter your details to create a new account.</p>
          </div>

          <form class="space-y-4" @submit.prevent="handleRegister">
            <div>
              <label for="fullName" class="mb-2 block text-sm font-medium text-slate-700">Full names</label>
              <input
                id="fullName"
                v-model.trim="fullName"
                type="text"
                autocomplete="name"
                placeholder="Enter your full names"
                class="w-full rounded-xl border border-slate-300 bg-white px-4 py-3 text-sm text-slate-800 outline-none transition focus:border-[#00696b] focus:ring-4 focus:ring-[#00696b]/10"
              />
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

            <div class="grid gap-4 sm:grid-cols-2">
              <div>
                <label for="password" class="mb-2 block text-sm font-medium text-slate-700">Password</label>
                <div class="relative">
                  <input
                    id="password"
                    v-model="password"
                    :type="showPassword ? 'text' : 'password'"
                    autocomplete="new-password"
                    placeholder="Create a password"
                    class="w-full rounded-xl border border-slate-300 bg-white px-4 py-3 pr-11 text-sm text-slate-800 outline-none transition focus:border-[#00696b] focus:ring-4 focus:ring-[#00696b]/10"
                  />
                  <button
                    type="button"
                    class="absolute right-3 top-1/2 -translate-y-1/2 text-slate-500 transition hover:text-[#00696b]"
                    :aria-label="showPassword ? 'Hide password' : 'Show password'"
                    @click="togglePasswordVisibility"
                  >
                    <svg v-if="!showPassword" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" class="h-5 w-5" stroke-width="1.8">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M2.25 12s3.75-7.5 9.75-7.5S21.75 12 21.75 12s-3.75 7.5-9.75 7.5S2.25 12 2.25 12Z" />
                      <circle cx="12" cy="12" r="3" />
                    </svg>
                    <svg v-else xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" class="h-5 w-5" stroke-width="1.8">
                      <path stroke-linecap="round" stroke-linejoin="round" d="M3 3l18 18" />
                      <path stroke-linecap="round" stroke-linejoin="round" d="M10.58 10.58a2 2 0 0 0 2.83 2.83" />
                      <path stroke-linecap="round" stroke-linejoin="round" d="M9.88 5.09A10.18 10.18 0 0 1 12 4.5c6 0 9.75 7.5 9.75 7.5a14.84 14.84 0 0 1-3.36 4.46M6.13 6.13A15.28 15.28 0 0 0 2.25 12s3.75 7.5 9.75 7.5a10.6 10.6 0 0 0 4.19-.86" />
                    </svg>
                  </button>
                </div>
                <ul class="mt-3 space-y-1 text-xs text-slate-600">
                  <li class="flex items-center gap-2">
                    <span
                      class="inline-flex h-4 w-4 items-center justify-center rounded-full border text-[10px] font-bold"
                      :class="hasMinLength ? 'border-emerald-600 bg-emerald-600 text-white' : 'border-slate-300 text-slate-400'"
                    >
                      <svg v-if="hasMinLength" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" class="h-3 w-3" stroke-width="3">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7" />
                      </svg>
                    </span>
                    At least 8 characters
                  </li>
                  <li class="flex items-center gap-2">
                    <span
                      class="inline-flex h-4 w-4 items-center justify-center rounded-full border text-[10px] font-bold"
                      :class="hasSpecialCharacter ? 'border-emerald-600 bg-emerald-600 text-white' : 'border-slate-300 text-slate-400'"
                    >
                      <svg v-if="hasSpecialCharacter" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="currentColor" class="h-3 w-3" stroke-width="3">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M5 13l4 4L19 7" />
                      </svg>
                    </span>
                    Includes a symbol or special character
                  </li>
                </ul>
              </div>

              <div>
                <label for="confirmPassword" class="mb-2 block text-sm font-medium text-slate-700">Confirm password</label>
                <input
                  id="confirmPassword"
                  v-model="confirmPassword"
                  type="password"
                  autocomplete="new-password"
                  placeholder="Re-enter your password"
                  class="w-full rounded-xl border border-slate-300 bg-white px-4 py-3 text-sm text-slate-800 outline-none transition focus:border-[#00696b] focus:ring-4 focus:ring-[#00696b]/10"
                />
              </div>
            </div>

            <label class="flex items-start gap-3 text-sm text-slate-600">
              <input
                v-model="agreedToTerms"
                type="checkbox"
                class="mt-1 h-4 w-4 rounded border-slate-300 text-[#00696b] focus:ring-[#00696b]"
              />
              <span>
                I agree to the
                <NuxtLink to="/terms" class="font-semibold text-[#00696b] hover:text-[#004d4f]">Terms of service</NuxtLink>
                and
                <NuxtLink to="/privacy-policy" class="font-semibold text-[#00696b] hover:text-[#004d4f]">Privacy policy</NuxtLink>.
              </span>
            </label>

            <p v-if="message" class="text-sm" :class="messageType === 'error' ? 'text-rose-600' : 'text-[#00696b]'">
              {{ message }}
            </p>

            <button
              type="submit"
              class="inline-flex w-full items-center justify-center rounded-xl bg-[#00696b] px-4 py-3 text-sm font-semibold text-white shadow-sm transition hover:bg-[#004d4f]"
            >
              Create account
            </button>
          </form>

          <p class="mt-5 text-center text-sm text-slate-500 lg:text-left">
            Already have an account?
            <NuxtLink to="/login" class="font-semibold text-[#00696b] hover:text-[#004d4f]">Sign in</NuxtLink>
          </p>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup lang="ts">
import { computed, ref } from 'vue'

type RegistrationProfile = {
  fullName: string
  email: string
  registeredAt: string
}

const REGISTRATION_STORAGE_KEY = 'serene-registration-profile'

const fullName = ref('')
const email = ref('')
const password = ref('')
const confirmPassword = ref('')
const agreedToTerms = ref(false)
const showPassword = ref(false)
const message = ref('')
const messageType = ref<'error' | 'success'>('success')

const hasMinLength = computed(() => password.value.length >= 8)
const hasSpecialCharacter = computed(() => /[^A-Za-z0-9]/.test(password.value))
const isPasswordStrong = computed(() => hasMinLength.value && hasSpecialCharacter.value)

function togglePasswordVisibility() {
  showPassword.value = !showPassword.value
}

function handleRegister() {
  message.value = ''

  if (!fullName.value || !email.value || !password.value || !confirmPassword.value) {
    messageType.value = 'error'
    message.value = 'Fill in all fields before creating your account.'
    return
  }

  if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email.value)) {
    messageType.value = 'error'
    message.value = 'Enter a valid email address.'
    return
  }

  if (!isPasswordStrong.value) {
    messageType.value = 'error'
    message.value = 'Use at least 8 characters and include a symbol or special character.'
    return
  }

  if (password.value !== confirmPassword.value) {
    messageType.value = 'error'
    message.value = 'Passwords do not match.'
    return
  }

  if (!agreedToTerms.value) {
    messageType.value = 'error'
    message.value = 'You must agree to the terms and privacy policy to continue.'
    return
  }

  messageType.value = 'success'
  message.value = 'Account created. Redirecting to sign in...'

  if (import.meta.client) {
    const profile: RegistrationProfile = {
      fullName: fullName.value,
      email: email.value,
      registeredAt: new Date().toISOString()
    }

    localStorage.setItem(REGISTRATION_STORAGE_KEY, JSON.stringify(profile))
  }

  navigateTo('/login')
}
</script>