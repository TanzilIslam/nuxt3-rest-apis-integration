<template>
  <NuxtLink :to="paymentInfo?.to">
    <div
      class="border shadow rounded-lg p-4 text-white"
      :class="`${paymentInfo.bg}`"
    >
      <h2 class="text-lg font-bold mb-2 text-center">
        {{ paymentInfo.title }}
      </h2>
    </div>
  </NuxtLink>
</template>

<script setup>
defineProps(['paymentInfo'])
</script>

<style lang="scss" scoped></style>
