<template>
  <div class="min-h-screen bg-[radial-gradient(circle_at_top_left,_rgba(0,105,107,0.15),_transparent_34%),linear-gradient(180deg,_#eef5f4_0%,_#dce9e7_100%)] text-slate-800">
    <main class="mx-auto flex min-h-screen w-full max-w-6xl items-center px-4 py-10 sm:px-6 lg:px-8">
      <section class="grid w-full gap-8 rounded-[2rem] border border-white/70 bg-white/80 p-6 shadow-[0_20px_60px_rgba(15,23,42,0.1)] backdrop-blur md:grid-cols-[1.1fr_0.9fr] md:p-8">
        <div class="space-y-6">
          <p class="text-sm font-semibold uppercase tracking-[0.35em] text-[#00696b]">Serene Habitat</p>
          <h1 class="max-w-xl text-4xl font-bold tracking-tight text-slate-900 sm:text-5xl">
            One login for tenants, landlords, and admins.
          </h1>
          <p class="max-w-lg text-base leading-7 text-slate-500">
            Use the shared sign-in form to enter your email and password, then continue to the dashboard for your role.
          </p>

          <div class="flex flex-wrap gap-3">
            <NuxtLink to="/login" class="rounded-full bg-[#00696b] px-5 py-3 text-sm font-semibold text-white shadow-sm transition hover:bg-[#004d4f]">
              Go to Login
            </NuxtLink>
            <NuxtLink to="/tenant/dashboard" class="rounded-full border border-slate-300 px-5 py-3 text-sm font-semibold text-slate-700 transition hover:bg-slate-50">
              View Tenant Demo
            </NuxtLink>
          </div>
        </div>

        <div class="grid gap-4">
          <NuxtLink to="/login" class="rounded-2xl border border-slate-200 bg-white p-5 transition hover:border-[#00696b] hover:shadow-sm">
            <p class="text-sm font-semibold text-slate-900">Login form</p>
            <p class="mt-2 text-sm leading-6 text-slate-500">Enter email, password, and role in one place.</p>
          </NuxtLink>
          <NuxtLink to="/tenant/dashboard" class="rounded-2xl border border-slate-200 bg-white p-5 transition hover:border-[#00696b] hover:shadow-sm">
            <p class="text-sm font-semibold text-slate-900">Tenant dashboard</p>
            <p class="mt-2 text-sm leading-6 text-slate-500">Payments, rewards, and property details.</p>
          </NuxtLink>
          <NuxtLink to="/landlord/dashboard" class="rounded-2xl border border-slate-200 bg-white p-5 transition hover:border-[#00696b] hover:shadow-sm">
            <p class="text-sm font-semibold text-slate-900">Landlord dashboard</p>
            <p class="mt-2 text-sm leading-6 text-slate-500">Collections, tickets, and unit controls.</p>
          </NuxtLink>
          <NuxtLink to="/admin/dashboard" class="rounded-2xl border border-slate-200 bg-white p-5 transition hover:border-[#00696b] hover:shadow-sm">
            <p class="text-sm font-semibold text-slate-900">Admin dashboard</p>
            <p class="mt-2 text-sm leading-6 text-slate-500">Occupancy and system status at a glance.</p>
          </NuxtLink>
        </div>
      </section>
    </main>
  </div>
</template>
