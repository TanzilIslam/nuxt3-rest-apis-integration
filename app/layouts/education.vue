<script setup>
import { educationIntegrationLinks } from '~/utils/links'
</script>
<template>
  <h1 class="text-2xl font-bold mb-4">Educational API Integration</h1>
  <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-10">
    <Cards
      v-for="(education, index) in educationIntegrationLinks"
      :key="index"
      :cardInfo="education"
    />
  </div>
  <slot />
</template>
