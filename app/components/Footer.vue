<template>
  <footer class="bg-gray-800 text-white p-4 mt-auto">
    <div class="container mx-auto flex justify-between items-center">
      <div class="text-lg font-bold">
        &copy; {{ new Date().getFullYear() }} My App
      </div>
      <ul class="flex space-x-4">
        <li v-for="(link, index) in footerLinks" :key="index">
          <NuxtLink
            :to="link.to"
            class="hover:text-gray-300 transition-colors"
            >{{ link.title }}</NuxtLink
          >
        </li>
      </ul>
    </div>
  </footer>
</template>

<script setup>
import { footerLinks } from '~/utils/links'
</script>

<style scoped></style>
