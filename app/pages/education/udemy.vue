<script setup>
import { udemyData } from '@/utils/staticData'
import Multiselect from 'vue-multiselect'
const nuxtApp = useNuxtApp()

// const fetchCoursesData = async (page = 1, pageSize = 10) => {
//   try {
//     const options = {
//       method: 'GET',
//       headers: {
//         'x-rapidapi-key': 'YOUR_API_KEY', // Replace with your API key
//         'x-rapidapi-host': 'udemy-paid-courses-for-free-api.p.rapidapi.com',
//       },
//       params: {
//         page: String(page),
//         page_size: String(pageSize),
//       },
//     }
//     const url = ``
//     return await $fetch(url, options)
//   } catch (error) {
//     console.error('Error fetching course data:', error)
//     return null
//   }
// }

// const transformCoursesData = (payload) => ({
//   ...payload,
//   fetchedAt: new Date(),
// })
// const getCachedData = (key) => {
//   const data = nuxtApp.payload.data[key] || nuxtApp.static.data[key]
//   if (!data) return null

//   const expiration = new Date(data.fetchedAt)
//   expiration.setTime(expiration.getTime() + 120 * 1000)
//   return expiration.getTime() > Date.now() ? data : null
// }

// const page = 1
// const { status, data } = await useLazyAsyncData(
//   async () => {
//     return await fetchCoursesData(page, 163)
//   },
//   {
//     transform: transformCoursesData,
//     getCachedData,
//   }
// )
const searchQuery = ref('')
const sortOption = ref('sale_end_asc')
const currentPage = ref(1)
const pageSize = 30
const status = 'ready'
const selectedCategory = ref(null)
const categories = computed(() =>
  Array.from(new Set(udemyData.courses.map((course) => course.category)))
)
const filteredCourses = computed(() => {
  const lowerCaseQuery = searchQuery.value.toLowerCase()

  return udemyData.courses.filter((course) => {
    const matchesSearch =
      course.name.toLowerCase().includes(lowerCaseQuery) ||
      course.category.toLowerCase().includes(lowerCaseQuery) ||
      course.description.toLowerCase().includes(lowerCaseQuery)

    const matchesCategory =
      !selectedCategory.value || course.category === selectedCategory.value

    return matchesSearch && matchesCategory
  })
})
const sortedCourses = computed(() => {
  const sortedData = [...filteredCourses.value]
  switch (sortOption.value) {
    case 'sale_end_asc':
      return sortedData.sort(
        (a, b) => new Date(a.sale_end) - new Date(b.sale_end)
      )
    case 'sale_end_desc':
      return sortedData.sort(
        (a, b) => new Date(b.sale_end) - new Date(a.sale_end)
      )
    case 'actual_price_usd_asc':
      return sortedData.sort((a, b) => a.actual_price_usd - b.actual_price_usd)
    case 'actual_price_usd_desc':
      return sortedData.sort((a, b) => b.actual_price_usd - a.actual_price_usd)
    default:
      return sortedData
  }
})
const paginatedCourses = computed(() => {
  const start = (currentPage.value - 1) * pageSize
  const end = start + pageSize
  return sortedCourses.value.slice(start, end)
})
const onPageChanged = (page) => {
  currentPage.value = page
}

watch([searchQuery, sortOption], () => {
  currentPage.value = 1
})
</script>

<template>
  <div>
    <h1 class="text-3xl font-bold mb-6 text-center">
      Udemy Courses({{ filteredCourses.length }})
    </h1>
    <div class="grid grid-cols-12 gap-4 mb-10">
      <div class="col-span-12 md:col-span-4">
        <input
          v-model="searchQuery"
          type="text"
          placeholder="Search by title, category, or description..."
          class="p-2 border rounded w-full outline-none"
        />
      </div>
      <div class="col-span-12 md:col-span-4">
        <select
          v-model="sortOption"
          class="p-2 border rounded w-48 outline-none w-full"
        >
          <option value="sale_end_asc">By Sale End (Asc)</option>
          <option value="sale_end_desc">By Sale End (Desc)</option>
          <option value="actual_price_usd_asc">By Actual Price (Asc)</option>
          <option value="actual_price_usd_desc">By Actual Price (Desc)</option>
        </select>
      </div>
      <div class="col-span-12 md:col-span-4">
        <multiselect
          v-model="selectedCategory"
          :options="categories"
          placeholder="Select Category"
        ></multiselect>
      </div>
    </div>

    <div v-if="status === 'loading'" class="text-center text-gray-500">
      Loading...
    </div>
    <div v-else-if="status === 'error'" class="text-center text-red-500">
      Failed to load data.
    </div>
    <div v-else class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
      <CourseCard
        v-for="(course, index) in paginatedCourses"
        :key="index"
        :course="course"
      />
    </div>
    <div class="mt-6 flex justify-center">
      <Pagination
        :currentPage="currentPage"
        :perPage="pageSize"
        :total="filteredCourses.length"
        @page-changed="onPageChanged"
      />
    </div>
  </div>
</template>
<style src="vue-multiselect/dist/vue-multiselect.min.css"></style>

<style scoped></style>
