<script setup>
import { paymentIntegrationLinks } from '~/utils/links'
</script>

<style lang="scss" scoped></style>
<template>
  <h1 class="text-2xl font-bold mb-4">Payment Integration</h1>
  <div class="grid grid-cols-1 md:grid-cols-6 gap-4">
    <Cards
      v-for="(paymentIntegration, index) in paymentIntegrationLinks"
      :key="index"
      :paymentInfo="paymentIntegration"
    />
  </div>
  <slot />
</template>
