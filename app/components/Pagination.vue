<template>
  <nav class="pagination">
    <ul class="flex flex-wrap gap-4">
      <!-- Previous button -->
      <li
        v-if="currentOffset > 0"
        @click="changeOffset(currentOffset - 1)"
        class="cursor-pointer px-3 py-1 border rounded"
      >
        ...
      </li>

      <!-- Page buttons -->
      <li
        v-for="page in visiblePages"
        :key="page"
        :class="{ active: page === currentPage }"
        @click="changePage(page)"
        class="cursor-pointer px-3 py-1 border rounded"
      >
        {{ page }}
      </li>

      <!-- Next button -->
      <li
        v-if="currentOffset + 1 < totalOffsets"
        @click="changeOffset(currentOffset + 1)"
        class="cursor-pointer px-3 py-1 border rounded"
      >
        ...
      </li>
    </ul>
  </nav>
</template>

<script setup>
const props = defineProps(['currentPage', 'perPage', 'total'])
const emit = defineEmits(['page-changed'])

// Pagination settings
const pagesPerView = 3 // Number of buttons visible at a time
const totalPages = computed(() => Math.ceil(props.total / props.perPage))
const totalOffsets = computed(() => Math.ceil(totalPages.value / pagesPerView))

// Offset for visible page range
const currentOffset = ref(0)

// Compute the visible pages based on the offset
const visiblePages = computed(() => {
  const startPage = currentOffset.value * pagesPerView + 1
  const endPage = Math.min(startPage + pagesPerView - 1, totalPages.value)
  return Array.from(
    { length: endPage - startPage + 1 },
    (_, i) => startPage + i
  )
})

// Emit event when a page is selected
const changePage = (page) => {
  emit('page-changed', page)
}

// Change the offset for the next or previous set of pages
const changeOffset = (offset) => {
  currentOffset.value = offset
}
</script>

<style scoped>
.active {
  background-color: #f3f4f6;
  font-weight: bold;
}
</style>
