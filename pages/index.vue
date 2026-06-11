<template>
  <div class="relative min-h-screen h-screen overflow-hidden bg-slate-950 text-white">
    <div class="absolute inset-0 h-full w-full bg-fixed bg-cover bg-center" style="background-image: url('https://images.unsplash.com/photo-1512453979798-5ea266f8880c?auto=format&fit=crop&w=1600&q=80');"></div>
    <div class="absolute inset-0 bg-slate-950/75"></div>

    <main class="relative mx-auto flex min-h-screen max-w-7xl flex-col justify-center px-6 py-10 sm:px-8 lg:px-12">
      <div class="max-w-3xl space-y-8">
        <span class="inline-flex items-center rounded-full bg-cyan-400/15 px-4 py-1 text-xs font-semibold uppercase tracking-[0.35em] text-cyan-200">
          Welcome to Rental Agency
        </span>
        <h1 class="text-5xl font-semibold tracking-tight text-white sm:text-6xl">
          Nairobi rental management with a beautiful city view.
        </h1>
        <p class="max-w-2xl text-lg leading-8 text-slate-200/90">
          Build a smooth rental experience for tenants, landlords, and admins with a landing page that opens with Nairobi’s energy.
        </p>

        <div class="flex flex-wrap gap-4">
          <NuxtLink
            to="/login"
            class="inline-flex items-center justify-center rounded-full bg-cyan-400 px-6 py-3 text-sm font-semibold text-slate-950 transition hover:bg-cyan-300"
          >
            Login
          </NuxtLink>
          <NuxtLink
            to="/register"
            class="inline-flex items-center justify-center rounded-full border border-white/30 bg-white/10 px-6 py-3 text-sm font-semibold text-white transition hover:border-cyan-300 hover:bg-white/20"
          >
            Register
          </NuxtLink>
        </div>
      </div>
    </main>
  </div>
</template>
