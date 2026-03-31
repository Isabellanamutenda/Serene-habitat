<template>
  <div class="flex items-center justify-center min-h-screen bg-[#dce9e7] text-slate-800">
    <div class="space-y-6 text-center">
      <h1 class="text-3xl font-bold">Welcome to Smart Habitat</h1>
      <p class="text-gray-400">Choose your dashboard to continue</p>

      <div class="space-x-4">
        <NuxtLink to="/tenant/dashboard" class="btn btn-primary">Tenant</NuxtLink>
        <NuxtLink to="/landlord/dashboard" class="btn btn-success">Landlord</NuxtLink>
        <NuxtLink to="/admin/dashboard" class="btn btn-primary">Admin</NuxtLink>
      </div>
    </div>
  </div>
</template>
