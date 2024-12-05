<template>
  <NuxtLink :to="cardInfo?.to">
    <div
      class="border shadow rounded-lg p-4 text-white h-full flex flex-col justify-center items-center"
      :class="`${cardInfo?.bg}`"
    >
      <h2 class="text-lg font-bold mb-2 text-center">
        {{ cardInfo?.title }}
      </h2>
    </div>
  </NuxtLink>
</template>

<script setup>
defineProps(['cardInfo'])
</script>
