<template>
  <div class="min-h-screen bg-[#dce9e7] text-slate-800 flex flex-col lg:flex-row">
    <LandlordSidebar />

    <div class="flex-1 flex flex-col">
      <main class="p-4 sm:p-6 flex-1">
        <section class="max-w-7xl mx-auto space-y-6">
          <header class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
            <div>
              <h1 class="text-3xl sm:text-4xl font-bold text-slate-900">Property Breakdown</h1>
              <p class="mt-2 max-w-2xl text-slate-500 text-sm sm:text-base">Showcasing fully furnished studio, 1 bedroom, and 2 bedroom apartments with a premium gallery feel.</p>
            </div>
            <NuxtLink
              to="/landlord/dashboard"
              class="inline-flex items-center justify-center rounded-lg border border-slate-300 px-4 py-2 text-sm font-semibold text-slate-700 hover:bg-slate-100 transition-colors"
            >
              Back to Dashboard
            </NuxtLink>
          </header>

          <section class="rounded-[2rem] border border-white/70 bg-white/80 p-5 sm:p-6 shadow-[0_24px_80px_-40px_rgba(15,23,42,0.45)] backdrop-blur space-y-6">
            <div class="flex items-center justify-between gap-4">
              <div class="flex items-center gap-3">
                <button
                  :class="['rounded-full px-4 py-2 text-sm font-semibold', activeTab === 'furnished' ? 'bg-[#00696b] text-white' : 'bg-white/60 text-slate-700']"
                  @click="activeTab = 'furnished'"
                >
                  Furnished
                </button>
                <button
                  :class="['rounded-full px-4 py-2 text-sm font-semibold', activeTab === 'unfurnished' ? 'bg-[#00696b] text-white' : 'bg-white/60 text-slate-700']"
                  @click="activeTab = 'unfurnished'"
                >
                  Unfurnished
                </button>
              </div>
            </div>

            <div class="grid gap-4 lg:grid-cols-[1.3fr_0.7fr]">
              <article class="relative overflow-hidden rounded-[1.75rem] min-h-[420px] bg-slate-900 shadow-xl">
                <img
                  :src="activeTab === 'furnished' ? heroProperty.image : heroProperty.unfurnishedImage"
                  :alt="heroProperty.title"
                  class="absolute inset-0 h-full w-full object-cover cursor-pointer"
                  loading="eager"
                  @click="openModal(heroProperty, activeTab)"
                />
                <div class="absolute inset-0 bg-gradient-to-r from-slate-950/90 via-slate-950/40 to-transparent"></div>
                <div class="absolute inset-0 p-6 sm:p-8 flex flex-col justify-between">
                  <div class="flex items-start justify-between gap-4">
                    <div class="inline-flex items-center rounded-full bg-white/12 px-4 py-2 text-xs font-semibold uppercase tracking-[0.24em] text-white/80 backdrop-blur-sm">
                      Featured Furnished Gallery
                    </div>
                    <div class="rounded-2xl bg-white/12 px-4 py-3 text-right text-white backdrop-blur-sm border border-white/15">
                      <p class="text-xs uppercase tracking-[0.2em] text-white/60">Starting from</p>
                      <p class="text-2xl font-semibold">{{ heroProperty.rent }}</p>
                    </div>
                  </div>

                    <div class="max-w-xl text-white space-y-4">
                    <p class="text-sm uppercase tracking-[0.3em] text-[#9de7e4]">{{ heroProperty.location }}</p>
                    <h2 class="text-3xl sm:text-5xl font-semibold leading-tight">{{ heroProperty.title }}</h2>
                    <p class="text-white/80 max-w-lg">{{ heroProperty.description }}</p>

                    <div class="flex flex-wrap gap-2">
                      <span
                        v-for="tag in heroProperty.tags"
                        :key="tag"
                        class="rounded-full border border-white/15 bg-white/10 px-3 py-1 text-xs font-medium text-white/90 backdrop-blur-sm"
                      >
                        {{ tag }}
                      </span>
                    </div>

                    <div class="grid gap-3 sm:grid-cols-3 pt-2">
                      <div class="rounded-2xl border border-white/10 bg-white/8 px-4 py-3 backdrop-blur-sm">
                        <p class="text-xs uppercase tracking-[0.2em] text-white/55">Bedrooms</p>
                        <p class="mt-1 text-xl font-semibold">{{ heroProperty.bedrooms }}</p>
                      </div>
                      <div class="rounded-2xl border border-white/10 bg-white/8 px-4 py-3 backdrop-blur-sm">
                        <p class="text-xs uppercase tracking-[0.2em] text-white/55">Bathrooms</p>
                        <p class="mt-1 text-xl font-semibold">{{ heroProperty.bathrooms }}</p>
                      </div>
                      <div class="rounded-2xl border border-white/10 bg-white/8 px-4 py-3 backdrop-blur-sm">
                        <p class="text-xs uppercase tracking-[0.2em] text-white/55">Area</p>
                        <p class="mt-1 text-xl font-semibold">{{ heroProperty.area }}</p>
                      </div>
                    </div>
                  </div>
                </div>
              </article>

              <div class="grid gap-4 sm:grid-cols-2 lg:grid-cols-1">
                <article
                  v-for="summary in summaryCards"
                  :key="summary.title"
                  class="rounded-[1.5rem] border border-slate-100 bg-slate-50/80 p-5 shadow-sm"
                >
                  <div class="flex items-center justify-between gap-4">
                    <div>
                      <p class="text-xs uppercase tracking-[0.24em] text-slate-400">{{ summary.label }}</p>
                      <h3 class="mt-2 text-2xl font-semibold text-slate-900">{{ summary.title }}</h3>
                    </div>
                    <span class="rounded-full bg-white px-3 py-1 text-xs font-semibold text-[#00696b] shadow-sm">{{ summary.badge }}</span>
                  </div>

                  <div class="mt-4 grid grid-cols-2 gap-3">
                    <div class="rounded-2xl bg-white p-3 shadow-sm">
                      <p class="text-xs uppercase tracking-[0.2em] text-slate-400">Beds</p>
                      <p class="mt-1 text-lg font-semibold text-slate-900">{{ summary.bedrooms }}</p>
                    </div>
                    <div class="rounded-2xl bg-white p-3 shadow-sm">
                      <p class="text-xs uppercase tracking-[0.2em] text-slate-400">Monthly</p>
                      <p class="mt-1 text-lg font-semibold text-slate-900">{{ summary.rent }}</p>
                    </div>
                  </div>

                  <p class="mt-4 text-sm leading-6 text-slate-600">{{ summary.description }}</p>
                </article>
              </div>
            </div>

            <div class="grid gap-5 md:grid-cols-3">
              <article
                v-for="property in properties"
                :key="property.title"
                class="group overflow-hidden rounded-[1.75rem] border border-slate-100 bg-white shadow-[0_18px_50px_-30px_rgba(15,23,42,0.5)] transition-transform duration-300 hover:-translate-y-1 cursor-pointer"
                @click="openModal(property, activeTab)"
              >
                <div class="relative aspect-[4/5] overflow-hidden">
                  <img
                    :src="activeTab === 'furnished' ? property.image : property.unfurnishedImage"
                    :alt="property.title"
                    class="h-full w-full object-cover transition-transform duration-500 group-hover:scale-105"
                    loading="lazy"
                  />
                  <div class="absolute inset-0 bg-gradient-to-t from-slate-950/85 via-slate-950/20 to-transparent"></div>
                  <div class="absolute inset-x-0 bottom-0 p-5 text-white">
                    <div class="flex items-center justify-between gap-3">
                      <div>
                        <p class="text-xs uppercase tracking-[0.22em] text-white/65">{{ property.type }}</p>
                        <h3 class="mt-1 text-2xl font-semibold">{{ property.title }}</h3>
                      </div>
                      <p class="rounded-full bg-white/12 px-3 py-1 text-sm font-semibold backdrop-blur-sm">{{ property.rent }}</p>
                    </div>
                    <p class="mt-2 text-sm text-white/78">{{ property.location }}</p>
                  </div>
                </div>

                <div class="space-y-4 p-5">
                  <div class="flex flex-wrap gap-2">
                    <span
                      v-for="feature in property.features"
                      :key="feature"
                      class="rounded-full bg-slate-100 px-3 py-1 text-xs font-medium text-slate-600"
                    >
                      {{ feature }}
                    </span>
                  </div>

                  <p class="text-sm leading-6 text-slate-600">{{ property.description }}</p>

                  <div class="grid grid-cols-3 gap-3">
                    <div class="rounded-2xl bg-slate-50 p-3 text-center">
                      <p class="text-[11px] uppercase tracking-[0.18em] text-slate-400">Units</p>
                      <p class="mt-1 text-lg font-semibold text-slate-900">{{ property.units }}</p>
                    </div>
                    <div class="rounded-2xl bg-slate-50 p-3 text-center">
                      <p class="text-[11px] uppercase tracking-[0.18em] text-slate-400">Baths</p>
                      <p class="mt-1 text-lg font-semibold text-slate-900">{{ property.bathrooms }}</p>
                    </div>
                    <div class="rounded-2xl bg-slate-50 p-3 text-center">
                      <p class="text-[11px] uppercase tracking-[0.18em] text-slate-400">Area</p>
                      <p class="mt-1 text-lg font-semibold text-slate-900">{{ property.area }}</p>
                    </div>
                  </div>

                  <div v-if="activeTab === 'furnished'" class="rounded-2xl border border-[#cde9e8] bg-[#f2fbfb] p-4">
                    <p class="text-xs uppercase tracking-[0.22em] text-[#00696b]">Fully Furnished Includes</p>
                    <p class="mt-1 text-sm text-slate-700">{{ property.furnishings }}</p>
                  </div>
                  <div v-else class="rounded-2xl border border-slate-100 bg-slate-50 p-4">
                    <p class="text-xs uppercase tracking-[0.22em] text-slate-500">Unfurnished</p>
                    <p class="mt-1 text-sm text-slate-700">Empty shell — ready for your own furniture.</p>
                  </div>
                </div>
              </article>
            </div>
          </section>

          <div
            v-if="selectedProperty"
            class="fixed inset-0 z-50 bg-slate-900/50 backdrop-blur-sm flex items-center justify-center p-4"
            @click="closeProperty"
          >
            <div class="w-full max-w-3xl rounded-2xl bg-white text-slate-800 p-6 shadow-2xl" @click.stop>
              <div class="flex items-start justify-between gap-4">
                <div>
                  <h2 class="text-2xl font-bold text-slate-900">{{ selectedProperty.name }}</h2>
                  <p class="mt-1 text-sm text-slate-500">{{ selectedProperty.location }} · {{ selectedProperty.units }} units</p>
                </div>
                <button
                  class="text-slate-500 hover:text-slate-900 transition-colors"
                  aria-label="Close property details"
                  @click="closeProperty"
                >
                  ✕
                </button>
              </div>

              <div class="mt-5 grid grid-cols-1 sm:grid-cols-3 gap-4">
                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Total Rooms</p>
                  <p class="mt-2 text-2xl font-semibold text-slate-900">{{ selectedProperty.details.totalRooms }}</p>
                </article>
                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Bedrooms</p>
                  <p class="mt-2 text-2xl font-semibold text-slate-900">{{ selectedProperty.details.bedrooms }}</p>
                </article>
                <article class="rounded-xl border border-slate-100 bg-slate-50 p-4">
                  <p class="text-xs uppercase tracking-wide text-slate-500">Ensuite Units</p>
                  <p class="mt-2 text-2xl font-semibold text-slate-900">{{ selectedProperty.details.ensuiteUnits }}</p>
                </article>
              </div>

              <div class="mt-6 grid grid-cols-1 lg:grid-cols-2 gap-4">
                <section class="rounded-xl border border-slate-100 p-4">
                  <h3 class="text-lg font-semibold text-slate-900">Unit Breakdown</h3>
                  <ul class="mt-3 space-y-2 text-sm text-slate-700">
                    <li v-for="item in selectedProperty.details.roomTypes" :key="item.label" class="flex items-center justify-between border-b border-slate-100 pb-2 last:border-b-0 last:pb-0">
                      <span>{{ item.label }}</span>
                      <span class="font-semibold text-slate-900">{{ item.count }}</span>
                    </li>
                  </ul>
                </section>

                <section class="rounded-xl border border-slate-100 p-4">
                  <h3 class="text-lg font-semibold text-slate-900">More Details</h3>
                  <dl class="mt-3 space-y-3 text-sm">
                    <div class="flex items-center justify-between gap-4 border-b border-slate-100 pb-2">
                      <dt class="text-slate-500">Average Rent per Unit</dt>
                      <dd class="font-semibold text-slate-900">{{ selectedProperty.details.averageRent }}</dd>
                    </div>
                    <div class="flex items-center justify-between gap-4 border-b border-slate-100 pb-2">
                      <dt class="text-slate-500">Occupied Units</dt>
                      <dd class="font-semibold text-slate-900">{{ selectedProperty.details.occupiedUnits }}</dd>
                    </div>
                    <div class="flex items-center justify-between gap-4">
                      <dt class="text-slate-500">Vacant Units</dt>
                      <dd class="font-semibold text-slate-900">{{ selectedProperty.details.vacantUnits }}</dd>
                    </div>
                  </dl>
                </section>
              </div>
            </div>
          </div>
        </section>
      </main>
    </div>
    </div>

    <div v-if="showModal" class="fixed inset-0 z-50 flex items-center justify-center p-6">
      <div class="absolute inset-0 bg-black/60" @click="closeModal"></div>
      <div class="relative z-10 max-w-4xl w-full bg-white rounded-2xl overflow-hidden shadow-xl">
        <div class="flex items-center justify-between p-4 border-b">
          <h3 class="text-lg font-semibold">{{ modalTitle }}</h3>
          <button class="text-slate-600" @click="closeModal">Close</button>
        </div>
        <div class="p-4 bg-slate-50">
          <div v-if="modalProperty?.variants?.length" class="mb-4 flex flex-wrap gap-2">
            <button
              v-for="variant in modalProperty.variants"
              :key="variant.key"
              class="rounded-full px-4 py-2 text-sm font-semibold transition-colors"
              :class="activeVariantKey === variant.key ? 'bg-[#00696b] text-white' : 'bg-white text-slate-600 border border-slate-200'"
              @click="selectVariant(variant.key)"
            >
              {{ variant.label }}
            </button>
          </div>

          <div class="relative">
            <img :src="modalItems[modalIndex]" class="w-full h-[58vh] object-cover rounded-md" />
            <button class="absolute left-3 top-1/2 -translate-y-1/2 rounded-full bg-white/80 p-2 shadow" @click="prevImage">‹</button>
            <button class="absolute right-3 top-1/2 -translate-y-1/2 rounded-full bg-white/80 p-2 shadow" @click="nextImage">›</button>
          </div>

          <div class="mt-3 grid grid-cols-4 gap-2">
            <img
              v-for="(img, i) in modalItems"
              :key="img"
              :src="img"
              class="h-20 w-full object-cover rounded cursor-pointer border-2"
              :class="{'border-[#00696b]': modalIndex === i}"
              @click="modalIndex = i"
            />
          </div>
        </div>
      </div>
    </div>
  </template>

  <script setup lang="ts">
  import { ref } from 'vue'
  import LandlordSidebar from '~/components/landlord/LandlordSidebar.vue'

  const nairobiImages = {
    furnished: {
      studio: [
        'https://images.unsplash.com/photo-1658218729615-167c32d70537?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
        'https://images.unsplash.com/photo-1658218635253-64728f6234be?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
        'https://images.unsplash.com/photo-1625667782817-228e40c66aba?q=80&w=1752&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
      ],
      oneBedroom: [
        'https://images.unsplash.com/photo-1658218635253-64728f6234be?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
        'https://images.unsplash.com/photo-1658218729615-167c32d70537?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
        'https://images.unsplash.com/photo-1625667782817-228e40c66aba?q=80&w=1752&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
      ],
      twoBedroom: [
        'https://images.unsplash.com/photo-1658218635253-64728f6234be?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
        'https://images.unsplash.com/photo-1658218729615-167c32d70537?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
        'https://images.unsplash.com/photo-1625667782817-228e40c66aba?q=80&w=1752&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
      ]
    },
    unfurnished: {
      studio: ['https://images.unsplash.com/photo-1484154218962-a197022b5858?auto=format&fit=crop&w=1200&q=80'],
      oneBedroom: ['https://images.unsplash.com/photo-1505693416388-ac5ce068fe85?auto=format&fit=crop&w=1600&q=80'],
      twoBedroom: ['https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=1200&q=80']
    }
  }

const heroProperty = {
  title: 'Riverside Suites',
  location: 'Westlands, Nairobi',
  rent: 'Ksh 45,000',
  bedrooms: 'Studio',
  bathrooms: '1',
  area: '34 sqm',
  image: 'https://images.unsplash.com/photo-1658218729615-167c32d70537?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
  gallery: [
    'https://images.unsplash.com/photo-1658218729615-167c32d70537?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    'https://images.unsplash.com/photo-1658218635253-64728f6234be?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    'https://images.unsplash.com/photo-1625667782817-228e40c66aba?q=80&w=1752&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
  ],
  unfurnishedImage: 'https://images.unsplash.com/photo-1505693416388-ac5ce068fe85?auto=format&fit=crop&w=1600&q=80',
  unfurnishedGallery: [
    'https://images.unsplash.com/photo-1505693416388-ac5ce068fe85?auto=format&fit=crop&w=1600&q=80'
  ],
  tags: ['Fully furnished', 'Soft lighting', 'City view', 'Move-in ready']
}

const summaryCards = [
  {
    label: 'Riverside Suites',
    title: 'Studio apartment, polished and compact',
    badge: 'Most affordable',
    bedrooms: 'Studio',
    rent: 'Ksh 45,000',
    description: 'Warm textures, a smart kitchenette, and a calm sleeping nook make this the perfect entry-level furnished home in Westlands.'
  },
  {
    label: 'Downtown Plaza',
    title: '1 bedroom with a private lounge',
    badge: 'Best value',
    bedrooms: '1 Bed',
    rent: 'Ksh 68,000',
    description: 'A separate lounge, generous wardrobe space, and hotel-style furnishings create a refined everyday experience in the CBD.'
  }
]

const properties = [
  {
    type: 'Studio Apartment',
    title: 'Riverside Suites',
    location: 'Westlands',
    units: 18,
    rent: 'Ksh 45,000',
    details: {
      totalRooms: 54,
      bedrooms: 36,
      ensuiteUnits: 12,
      roomTypes: [
        { label: 'Studio units', count: 6 },
        { label: '1-bedroom units', count: 8 },
        { label: '2-bedroom units', count: 4 }
      ],
      averageRent: 'Ksh 66,700',
      occupiedUnits: 16,
      vacantUnits: 2
    },
    bedrooms: 'Studio',
    bathrooms: '1',
    area: '34 sqm',
    image: 'https://images.unsplash.com/photo-1658218729615-167c32d70537?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    gallery: [
      'https://images.unsplash.com/photo-1658218729615-167c32d70537?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
      'https://images.unsplash.com/photo-1658218635253-64728f6234be?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
      'https://images.unsplash.com/photo-1625667782817-228e40c66aba?q=80&w=1752&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
    ],
    unfurnishedImage: 'https://images.unsplash.com/photo-1484154218962-a197022b5858?auto=format&fit=crop&w=1200&q=80',
    unfurnishedGallery: [
      'https://images.unsplash.com/photo-1484154218962-a197022b5858?auto=format&fit=crop&w=1200&q=80'
    ],
    features: ['Queen bed', 'Kitchenette', 'Work desk'],
    furnishings: 'Queen bed, fitted wardrobe, marble-top kitchenette, study desk, accent chair, blackout curtains, and a wall-mounted TV.',
    description: 'Designed for a minimal footprint with a premium feel, this studio stays bright, airy, and beautifully furnished.'
  },
  {
    type: '1 Bedroom',
    title: 'Downtown Plaza',
    location: 'CBD, Nairobi',
    units: 22,
    rent: 'Ksh 68,000',
    details: {
      totalRooms: 66,
      bedrooms: 44,
      ensuiteUnits: 15,
      roomTypes: [
        { label: 'Studio units', count: 4 },
        { label: '1-bedroom units', count: 12 },
        { label: '2-bedroom units', count: 6 }
      ],
      averageRent: 'Ksh 68,200',
      occupiedUnits: 20,
      vacantUnits: 2
    },
    bedrooms: '1',
    bathrooms: '1',
    area: '58 sqm',
    image: 'https://images.unsplash.com/photo-1658218635253-64728f6234be?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    gallery: [
      'https://images.unsplash.com/photo-1658218635253-64728f6234be?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
      'https://images.unsplash.com/photo-1658218729615-167c32d70537?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
      'https://images.unsplash.com/photo-1625667782817-228e40c66aba?q=80&w=1752&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
    ],
    unfurnishedImage: 'https://images.unsplash.com/photo-1505693416388-ac5ce068fe85?auto=format&fit=crop&w=1600&q=80',
    unfurnishedGallery: [
      'https://images.unsplash.com/photo-1505693416388-ac5ce068fe85?auto=format&fit=crop&w=1600&q=80'
    ],
    variants: [
      { key: 'studio', label: 'Studio', furnishedGallery: nairobiImages.furnished.studio, unfurnishedGallery: nairobiImages.unfurnished.studio },
      { key: 'oneBedroom', label: '1 Bedroom', furnishedGallery: nairobiImages.furnished.oneBedroom, unfurnishedGallery: nairobiImages.unfurnished.oneBedroom },
      { key: 'twoBedroom', label: '2 Bedroom', furnishedGallery: nairobiImages.furnished.twoBedroom, unfurnishedGallery: nairobiImages.unfurnished.twoBedroom }
    ],
    defaultVariant: 'oneBedroom',
    features: ['Lounge seating', 'Dining nook', 'En-suite bath'],
    furnishings: 'Upholstered sofa set, dining table, king-size bed, bedside lamps, sleek storage, throw pillows, and a designer rug.',
    description: 'This one bedroom pairs a calm living room with a private sleeping area, creating a cozy but upscale furnished home.'
  },
  {
    type: '2 Bedroom',
    title: 'Green Valley',
    location: 'Kileleshwa, Nairobi',
    units: 14,
    rent: 'Ksh 95,000',
    details: {
      totalRooms: 42,
      bedrooms: 28,
      ensuiteUnits: 10,
      roomTypes: [
        { label: 'Studio units', count: 3 },
        { label: '1-bedroom units', count: 7 },
        { label: '2-bedroom units', count: 4 }
      ],
      averageRent: 'Ksh 70,000',
      occupiedUnits: 13,
      vacantUnits: 1
    },
    bedrooms: '2',
    bathrooms: '2',
    area: '92 sqm',
    image: 'https://images.unsplash.com/photo-1658218635253-64728f6234be?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
    gallery: [
      'https://images.unsplash.com/photo-1658218635253-64728f6234be?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
      'https://images.unsplash.com/photo-1658218729615-167c32d70537?q=80&w=1738&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D',
      'https://images.unsplash.com/photo-1625667782817-228e40c66aba?q=80&w=1752&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
    ],
    unfurnishedImage: 'https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=1200&q=80',
    unfurnishedGallery: [
      'https://images.unsplash.com/photo-1600585154340-be6161a56a0c?auto=format&fit=crop&w=1200&q=80'
    ],
    variants: [
      { key: 'studio', label: 'Studio', furnishedGallery: nairobiImages.furnished.studio, unfurnishedGallery: nairobiImages.unfurnished.studio },
      { key: 'oneBedroom', label: '1 Bedroom', furnishedGallery: nairobiImages.furnished.oneBedroom, unfurnishedGallery: nairobiImages.unfurnished.oneBedroom },
      { key: 'twoBedroom', label: '2 Bedroom', furnishedGallery: nairobiImages.furnished.twoBedroom, unfurnishedGallery: nairobiImages.unfurnished.twoBedroom }
    ],
    defaultVariant: 'twoBedroom',
    features: ['Family lounge', 'Master suite', 'Dining area'],
    furnishings: 'Large sectional sofa, six-seater dining set, two complete bedrooms, soft area rugs, smart storage, and layered lighting.',
    description: 'Ideal for professionals, couples, or small families who want more breathing room without losing the furnished luxury feel.'
  }
]

// Modal and tab state
const activeTab = ref('furnished')
const showModal = ref(false)
const modalItems = ref([] as Array<string>)
const modalTitle = ref('')
const modalIndex = ref(0)
const modalProperty = ref<any>(null)
const activeVariantKey = ref('studio')

function nextImage() {
  if (!modalItems.value.length) return
  modalIndex.value = (modalIndex.value + 1) % modalItems.value.length
}

function prevImage() {
  if (!modalItems.value.length) return
  modalIndex.value = (modalIndex.value - 1 + modalItems.value.length) % modalItems.value.length
}

function getVariant(property: any, variantKey: string) {
  return (property.variants || []).find((variant: any) => variant.key === variantKey) || property.variants?.[0] || null
}

function loadVariantImages(property: any, tab: string, variantKey: string) {
  const variant = getVariant(property, variantKey)
  if (tab === 'furnished') {
    return variant?.furnishedGallery || property.gallery || [property.image]
  }
  return variant?.unfurnishedGallery || property.unfurnishedGallery || [property.unfurnishedImage || property.image]
}

function selectVariant(variantKey: string) {
  if (!modalProperty.value) return
  activeVariantKey.value = variantKey
  modalItems.value = loadVariantImages(modalProperty.value, activeTab.value, variantKey)
  modalIndex.value = 0
}

function openModal(property: any, tab: string) {
  modalProperty.value = property
  activeVariantKey.value = property.defaultVariant || property.variants?.[0]?.key || 'studio'
  modalTitle.value = property.title
  modalItems.value = loadVariantImages(property, tab, activeVariantKey.value)
  modalIndex.value = 0
  showModal.value = true
}

function closeModal() {
  showModal.value = false
  modalItems.value = []
  modalProperty.value = null
}

interface PropertyDetail {
  totalRooms: number
  bedrooms: number
  ensuiteUnits: number
  roomTypes: Array<{ label: string; count: number }>
  averageRent: string
  occupiedUnits: number
  vacantUnits: number
}

interface PropertyRow {
  name: string
  location: string
  units: number
  rent: string
  details: PropertyDetail
}

const selectedProperty = ref<PropertyRow | null>(null)

const openProperty = (property: PropertyRow) => {
  selectedProperty.value = property
}

const closeProperty = () => {
  selectedProperty.value = null
}
</script>
