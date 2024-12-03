<template>
  <NuxtLink
    class="text-surface shadow shadow-secondary-1 dark:bg-surface-dark dark:text-white block h-full"
    :to="course.url"
    target="_blank"
  >
    <div class="">
      <!-- Course Image -->
      <img
        :src="course.image"
        :alt="course.name"
        class="w-full h-48 object-cover"
      />
      <!-- Course Details -->
      <div class="p-4">
        <h2 class="text-xl font-bold mb-4">{{ course.name }}</h2>
        <div class="flex flex-wrap gap-4 items-center justify-between">
          <p
            class="text-sm bg-gray-900 dark:bg-gray-200 text-white dark:text-gray-900 w-fit px-4 py-1 rounded"
          >
            {{ course.category }}
          </p>
          <p class="flex items-center text-lg text-gray-800 mb-4">
            <span class="font-bold text-red-500 text-xl mr-2"
              >${{ course.sale_price_usd }}</span
            >
            <span class="text-sm text-gray-500 line-through"
              >${{ course.actual_price_usd }}</span
            >
          </p>
        </div>

        <div class="flex flex-wrap gap-1 items-center mb-4">
          <BaseIcon name="clock" icon-class="h-4 w-4" />
          <p>Sale Ends:</p>
          <p class="text-sm">
            {{ getRemainingTime(course.sale_end) }}
          </p>
        </div>

        <p
          class="text-sm text-gray-600 dark:text-gray-100 line-clamp-6"
          v-html="course.description"
        ></p>
      </div>
      <!-- Course Link -->
    </div>
  </NuxtLink>
</template>

<script setup>
import { formatDistanceToNow } from 'date-fns'
defineProps({
  course: {
    type: Object,
    required: true,
  },
})
const getRemainingTime = (saleEndDate) => {
  return formatDistanceToNow(new Date(saleEndDate), { addSuffix: true })
}
</script>

<style scoped>
/* Scoped styles for specific customizations */
</style>
