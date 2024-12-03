<template>
  <nav class="bg-gray-800 text-white p-4" aria-label="Main Navigation">
    <div class="container mx-auto flex justify-between items-center">
      <!-- Logo -->
      <div class="text-2xl font-bold flex items-center">
        <Icon name="heroicons:rocket-launch" class="mr-2" />
        <NuxtLink
          to="/"
          class="text-white hover:text-gray-300 transition-colors"
          >Logo</NuxtLink
        >
      </div>

      <!-- Desktop Navigation -->
      <div class="hidden md:flex space-x-4">
        <NavLinks v-for="link in headerLinks" :key="link.to" :link="link" />
        <ThemeToggle />
      </div>

      <!-- Mobile Menu Toggle -->
      <button
        @click="toggleMenu"
        class="md:hidden focus:outline-none"
        aria-label="Toggle mobile menu"
        aria-controls="mobile-menu"
      >
        <BaseIcon :name="isMenuOpen ? 'x-mark' : 'bars-3'" />
      </button>
    </div>

    <!-- Mobile Menu -->
    <div
      v-if="isMenuOpen"
      id="mobile-menu"
      class="md:hidden fixed inset-x-0 top-16 bg-gray-800 z-50"
      aria-labelledby="mobile-menu-toggle"
    >
      <div class="flex flex-col space-y-2 p-4">
        <NavLinks
          v-for="link in headerLinks"
          :key="link.to"
          :link="link"
          @click="isMenuOpen = false"
        />
      </div>
    </div>
  </nav>
</template>

<script setup>
import NavLinks from '~/components/header/NavLinks'
import ThemeToggle from '~/components/header/ThemeToggle.vue'
import BaseIcon from '~/components/BaseIcon.vue'
import { headerLinks } from '~/utils/links'

const isMenuOpen = ref(false)

const toggleMenu = () => {
  isMenuOpen.value = !isMenuOpen.value
}
</script>
