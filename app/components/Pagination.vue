<template>
  <nav class="pagination">
    <ul class="flex space-x-2">
      <li
        v-for="page in totalPages"
        :key="page"
        :class="{ active: page === currentPage }"
        @click="changePage(page)"
        class="cursor-pointer px-3 py-1 border rounded"
      >
        {{ page }}
      </li>
    </ul>
  </nav>
</template>

<script setup>
const props = defineProps(['currentPage', 'perPage', 'total'])
const emit = defineEmits(['page-changed'])

const totalPages = computed(() => Math.ceil(props.total / props.perPage))

const changePage = (page) => {
  emit('page-changed', page)
}
</script>

<style scoped>
.active {
  background-color: #f3f4f6;
}
</style>
