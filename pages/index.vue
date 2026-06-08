<template>
  <div class="min-h-screen bg-slate-950 text-white">
    <main class="relative overflow-hidden">
      <section class="relative min-h-screen">
        <div class="absolute inset-0">
          <div
            v-for="(slide, index) in slides"
            :key="slide.label"
            class="absolute inset-0 transition-opacity duration-1000"
            :class="index === currentSlide ? 'opacity-100' : 'opacity-0'">
            <div
              class="absolute inset-0 bg-cover bg-center"
              :style="`background-image: url(${slide.image});`"
            />
            <div class="absolute inset-0 bg-slate-950/55 backdrop-blur-sm" />
          </div>
        </div>

        <div class="relative z-10 mx-auto flex min-h-screen max-w-7xl flex-col justify-between px-6 py-8 sm:px-10 lg:px-16">
          <header class="flex items-center justify-between gap-6">
            <div>
              <p class="text-sm font-semibold uppercase tracking-[0.35em] text-cyan-300/90">Serene Habitat</p>
            </div>
            <div class="flex items-center gap-3 text-sm text-slate-200">
              <NuxtLink to="/login" class="rounded-full border border-slate-200/30 bg-white/10 px-4 py-2 transition hover:border-cyan-300/80 hover:bg-white/15">
                Login
              </NuxtLink>
              <NuxtLink to="/register" class="rounded-full bg-cyan-400 px-4 py-2 font-semibold text-slate-950 transition hover:bg-cyan-300">
                Sign up
              </NuxtLink>
            </div>
          </header>

          <div class="grid gap-12 py-14 lg:grid-cols-[1.2fr_0.8fr] lg:items-center">
            <div class="max-w-2xl space-y-6">
              <p class="text-sm font-semibold uppercase tracking-[0.35em] text-cyan-200/90">Nairobi City Living</p>
              <h1 class="text-5xl font-semibold tracking-tight text-white sm:text-6xl">
                Build calm, modern rental experiences inspired by Nairobi.
              </h1>
              <p class="max-w-xl text-lg leading-8 text-slate-200/90">
                Showcase your housing platform with a stunning Nairobi skyline background, seamless role-based dashboards,
                and intuitive tools for tenants, landlords, and admins.
              </p>

              <div class="flex flex-wrap gap-4">
                <NuxtLink
                  to="/register"
                  class="inline-flex items-center justify-center rounded-full bg-cyan-400 px-6 py-3 text-sm font-semibold text-slate-950 shadow-lg shadow-cyan-500/20 transition hover:bg-cyan-300"
                >
                  Start free trial
                </NuxtLink>
                <NuxtLink
                  to="/tenant/dashboard"
                  class="inline-flex items-center justify-center rounded-full border border-white/30 bg-white/10 px-6 py-3 text-sm font-semibold text-white transition hover:border-cyan-300/80 hover:bg-white/15"
                >
                  Explore demo
                </NuxtLink>
              </div>
            </div>

            <div class="rounded-[2rem] border border-white/15 bg-slate-950/60 p-6 shadow-[0_30px_80px_rgba(15,23,42,0.65)] backdrop-blur-xl">
              <p class="text-sm font-semibold uppercase tracking-[0.35em] text-cyan-200/80">Featured Nairobi views</p>
              <div class="mt-6 space-y-5">
                <div class="rounded-3xl border border-white/10 bg-white/5 p-5">
                  <p class="text-sm uppercase tracking-[0.3em] text-slate-300">Skyline</p>
                  <p class="mt-2 text-xl font-semibold text-white">Westlands sunset & city glow</p>
                </div>
                <div class="rounded-3xl border border-white/10 bg-white/5 p-5">
                  <p class="text-sm uppercase tracking-[0.3em] text-slate-300">Lifestyle</p>
                  <p class="mt-2 text-xl font-semibold text-white">Urban living with calm property control</p>
                </div>
                <div class="rounded-3xl border border-white/10 bg-white/5 p-5">
                  <p class="text-sm uppercase tracking-[0.3em] text-slate-300">Trust</p>
                  <p class="mt-2 text-xl font-semibold text-white">Secure logins for tenants, landlords, and admins</p>
                </div>
              </div>
            </div>
          </div>

          <div class="mt-4 flex flex-col items-center gap-3 text-slate-200 sm:flex-row sm:justify-between">
            <div class="flex items-center gap-3">
              <span class="rounded-full bg-cyan-300/15 px-3 py-1 text-xs uppercase tracking-[0.24em] text-cyan-100">Live Nairobi carousel</span>
              <span class="text-sm text-slate-300">A curated cityscape for your landing page.</span>
            </div>

            <div class="flex items-center gap-2">
              <button
                v-for="(slide, index) in slides"
                :key="slide.label + '-dot'"
                type="button"
                @click="setSlide(index)"
                :class="[
                  'h-3 w-3 rounded-full transition-all duration-300',
                  index === currentSlide ? 'bg-cyan-300 w-8' : 'bg-white/30 hover:bg-white/50'
                ]"
              />
            </div>
          </div>
        </div>
      </section>

      <section class="bg-slate-950/95 py-16">
        <div class="mx-auto grid max-w-6xl gap-8 px-6 sm:px-10 lg:grid-cols-3 lg:px-0">
          <div class="rounded-[2rem] border border-white/10 bg-slate-900/80 p-8 shadow-xl shadow-slate-900/30">
            <p class="text-sm uppercase tracking-[0.35em] text-cyan-300/90">Grow with ease</p>
            <h2 class="mt-4 text-3xl font-semibold text-white">Manage every role</h2>
            <p class="mt-4 text-slate-300">Tenants, landlords, and admins all connect through one polished platform built for Nairobi’s modern rental market.</p>
          </div>
          <div class="rounded-[2rem] border border-white/10 bg-slate-900/80 p-8 shadow-xl shadow-slate-900/30">
            <p class="text-sm uppercase tracking-[0.35em] text-cyan-300/90">See the results</p>
            <h2 class="mt-4 text-3xl font-semibold text-white">Beautiful dashboards</h2>
            <p class="mt-4 text-slate-300">Showcase payment history, unit breakdowns, and maintenance ticket status with calm, data-driven visuals.</p>
          </div>
          <div class="rounded-[2rem] border border-white/10 bg-slate-900/80 p-8 shadow-xl shadow-slate-900/30">
            <p class="text-sm uppercase tracking-[0.35em] text-cyan-300/90">Launch faster</p>
            <h2 class="mt-4 text-3xl font-semibold text-white">Ready for Nairobi</h2>
            <p class="mt-4 text-slate-300">A landing page designed around the city’s energy, with a luxe visual identity and fast navigation to your app.</p>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<script setup lang="ts">
import { onMounted, onUnmounted, ref } from 'vue'

const slides = [
  {
    label: 'Nairobi skyline',
    image: 'https://images.unsplash.com/photo-1557683316-973673baf926?auto=format&fit=crop&w=1600&q=80',
  },
  {
    label: 'Urban district',
    image: 'https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?auto=format&fit=crop&w=1600&q=80',
  },
  {
    label: 'Downtown lights',
    image: 'https://images.unsplash.com/photo-1512453979798-5ea266f8880c?auto=format&fit=crop&w=1600&q=80',
  },
]

const currentSlide = ref(0)
let intervalId: ReturnType<typeof setInterval> | null = null

const setSlide = (index: number) => {
  currentSlide.value = index
}

onMounted(() => {
  intervalId = setInterval(() => {
    currentSlide.value = (currentSlide.value + 1) % slides.length
  }, 6000)
})

onUnmounted(() => {
  if (intervalId) {
    clearInterval(intervalId)
  }
})
</script>
