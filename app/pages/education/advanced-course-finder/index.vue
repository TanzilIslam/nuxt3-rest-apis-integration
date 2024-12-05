<template>
  <div>
    <div class="grid grid-col-12 md:grid-cols-4 gap-4">
      <div>
        <label for="search" class="block text-sm font-medium text-gray-700 mb-1"
          >Search</label
        >
        <input
          v-model="searchQuery"
          placeholder="Search"
          id="search"
          type="text"
          class="w-full outline-none px-4 py-2 rounded border"
        />
      </div>
      <div>
        <label
          for="country"
          class="block text-sm font-medium text-gray-700 mb-1"
          >Select Country ({{ countries.length }})</label
        >
        <Multiselect
          id="country"
          v-model="selectedCountry"
          :options="countries"
          :searchable="true"
          :clear-on-select="true"
          :close-on-select="true"
          placeholder="Choose a country"
          label="name"
          track-by="id"
          @update:modelValue="selectedState = null"
        />
      </div>
      <div>
        <label for="state" class="block text-sm font-medium text-gray-700 mb-1"
          >Select state ({{ states.length }})</label
        >
        <Multiselect
          id="state"
          v-model="selectedState"
          :options="states"
          :searchable="true"
          :clear-on-select="true"
          :close-on-select="true"
          placeholder="Choose a state"
          label="name"
          track-by="id"
        />
      </div>
      <div>
        <label for="sort" class="block text-sm font-medium text-gray-700 mb-1"
          >Sort List</label
        >
        <Multiselect
          id="sort"
          v-model="selectedSort"
          :options="universitySortOptions"
          :searchable="true"
          :clear-on-select="true"
          :close-on-select="true"
          placeholder="Sort"
          label="label"
          track-by="value"
          group-values="options"
          group-label="category"
        />
      </div>
    </div>
    <div class="flex justify-end items-center mt-8 mb-4">
      <button
        :class="`font-bold py-1 px-4 rounded ${
          currentView === 'university'
            ? 'bg-blue-500 hover:bg-blue-700 text-white'
            : ''
        }`"
        @click="currentView = 'university'"
      >
        University
      </button>
      <button
        :class="`font-bold py-1 px-4 rounded ${
          currentView === 'course'
            ? 'bg-blue-500 hover:bg-blue-700 text-white'
            : ''
        }`"
        @click="currentView = 'course'"
      >
        Course
      </button>
    </div>

    <div v-if="currentView === 'university'" class="space-y-8">
      <p class="text-2xl font-bold mb-4">
        University({{ filteredUniversities.length }})
      </p>
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
        <UniversityCard
          v-for="(university, index) in paginatedUniversities"
          :key="index"
          :university="university"
        />
      </div>
      <div class="flex justify-center">
        <Pagination
          :currentPage="currentPage"
          :perPage="itemsPerPage"
          :total="filteredUniversities.length"
          @page-changed="onPageChanged"
        />
      </div>
    </div>
    <div v-else class="space-y-8">
      <p class="text-2xl font-bold mb-4">
        Courses({{ filteredCourses.length }})
      </p>
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
        <UniversityCourseCard
          v-for="(Course, index) in paginatedCourse"
          :key="index"
          :course="Course"
        />
      </div>
      <div class="flex justify-center">
        <Pagination
          :currentPage="currentPageCourse"
          :perPage="itemsPerPage"
          :total="filteredCourses.length"
          @page-changed="onCoursePageChanged"
        />
      </div>
    </div>
  </div>
</template>

<script setup>
import { universityList, courseList } from '@/utils/course'
import Multiselect from 'vue-multiselect'

const universitySortOptions = [
  {
    category: 'University Type',
    options: [
      { label: 'Private', value: 'private' },
      { label: 'Public', value: 'public' },
    ],
  },
  {
    category: 'Has Scholarship',
    options: [
      { label: 'Scholarship', value: 'scholarship' },
      { label: 'No Scholarship', value: 'no-scholarship' },
    ],
  },
  {
    category: 'Ranking-Based Sorting',
    options: [
      { label: 'QS Ranking (Low to High)', value: 'qs-ranking-asc' },
      { label: 'QS Ranking (High to Low)', value: 'qs-ranking-desc' },
      { label: 'THE Ranking (Low to High)', value: 'the-ranking-asc' },
      { label: 'THE Ranking (High to Low)', value: 'the-ranking-desc' },
    ],
  },
  {
    category: 'Fee-Based Sorting',
    options: [
      { label: 'Tuition Fee (Low to High)', value: 'tuition-fee-asc' },
      { label: 'Tuition Fee (High to Low)', value: 'tuition-fee-desc' },
      { label: 'Living Fee (Low to High)', value: 'living-fee-asc' },
      { label: 'Living Fee (High to Low)', value: 'living-fee-desc' },
      { label: 'Other Fees (Low to High)', value: 'other-fee-asc' },
      { label: 'Other Fees (High to Low)', value: 'other-fee-desc' },
    ],
  },
  {
    category: 'Application Fee',
    options: [
      { label: 'Application Fee (Low to High)', value: 'app-fee-asc' },
      { label: 'Application Fee (High to Low)', value: 'app-fee-desc' },
    ],
  },
]

const currentView = ref('university')
const selectedCountry = ref(null)
const selectedState = ref(null)
const searchQuery = ref('')
const selectedSort = ref({
  label: 'THE Ranking (Low to High)',
  value: 'the-ranking-asc',
})
const currentPage = ref(1)
const currentPageCourse = ref(1)
const itemsPerPage = 6

const countries = computed(() => {
  const seen = new Set()
  return universityList
    .filter((c) => {
      if (seen.has(c.country_id)) {
        return false // Skip duplicates
      }
      seen.add(c.country_id)
      return true // Include unique country
    })
    .map((c) => ({
      id: c?.country_id,
      name: c?.country_name,
    }))
})
const states = computed(() => {
  if (!selectedCountry.value) return [] // Return an empty array if no country is selected

  const filteredStates = universityList
    .filter((c) => c.country_id === selectedCountry.value.id)
    .map((s) => ({
      id: s.state_id,
      name: s?.state_name,
    }))

  // Filter out duplicates based on the `id`
  const uniqueStates = filteredStates.filter(
    (value, index, self) => index === self.findIndex((t) => t.id === value.id)
  )

  return uniqueStates
})
const filteredUniversities = computed(() => {
  return universityList
    .filter((university) => {
      const matchesSearchQuery = university.university_name
        .toLowerCase()
        .includes(searchQuery.value.toLowerCase())
      const matchesCountry =
        !selectedCountry.value ||
        university.country_id === selectedCountry.value.id
      const matchesState =
        !selectedState.value || university.state_id === selectedState.value.id
      return matchesSearchQuery && matchesCountry && matchesState
    })
    .sort((a, b) => {
      if (selectedSort.value) {
        const sortField = `${selectedSort.value.value.split('-')[0]}-${
          selectedSort.value.value.split('-')[1]
        }`
        const multiplier =
          selectedSort.value.value.split('-')[2] === 'asc' ? 1 : -1
        switch (sortField) {
          case 'qs-ranking': {
            const qsRankingA = Number(a.qs_ranking)
            const qsRankingB = Number(b.qs_ranking)
            return (qsRankingA - qsRankingB) * multiplier
          }
          case 'the-ranking': {
            const theRankingA = Number(a.the_ranking)
            const theRankingB = Number(b.the_ranking)
            return (theRankingA - theRankingB) * multiplier
          }
          case 'tuition-fee': {
            const tuitionFeeA = Number(a.tution_fee)
            const tuitionFeeB = Number(b.tution_fee)
            return (tuitionFeeA - tuitionFeeB) * multiplier
          }
          case 'living-fee': {
            const livingFeeA = Number(a.living_fee)
            const livingFeeB = Number(b.living_fee)
            return (livingFeeA - livingFeeB) * multiplier
          }
          case 'other-fee': {
            const otherFeeA = Number(a.other_fee)
            const otherFeeB = Number(b.other_fee)
            return (otherFeeA - otherFeeB) * multiplier
          }

          case 'app-fee': {
            const appFeeA = Number(a.app_fee)
            const appFeeB = Number(b.app_fee)
            return (appFeeA - appFeeB) * multiplier
          }
          case 'private-undefined':
          case 'public-undefined': {
            const isPrivate = sortField === 'private-undefined'
            const typeA = a.type.toLowerCase()
            const typeB = b.type.toLowerCase()

            if (typeA === typeB) return 0 // If both types are the same, return 0
            return (
              (typeA === (!isPrivate ? 'private' : 'public') ? -1 : 1) *
              multiplier
            )
          }
          case 'private-undefined':
          case 'public-undefined': {
            const isPrivate = sortField === 'private-undefined'
            const typeA = a.type.toLowerCase()
            const typeB = b.type.toLowerCase()

            if (typeA === typeB) return 0 // If both types are the same, return 0
            return (
              (typeA === (!isPrivate ? 'private' : 'public') ? -1 : 1) *
              multiplier
            )
          }
          case 'scholarship-undefined':
          case 'no-scholarship': {
            const hasScholarship = sortField === 'scholarship-undefined'
            const scholarshipA = a.scholarship.toLowerCase()
            const scholarshipB = b.scholarship.toLowerCase()

            if (scholarshipA === scholarshipB) return 0 // If both have the same scholarship status, return 0
            return (
              (scholarshipA === (!hasScholarship ? 'yes' : 'no') ? -1 : 1) *
              multiplier
            )
          }
          default:
            return 0
        }
      }
      return 0
    })
})
const paginatedUniversities = computed(() => {
  const start = (currentPage.value - 1) * itemsPerPage
  return filteredUniversities.value.slice(start, start + itemsPerPage)
})
const filteredCourses = computed(() => {
  return courseList
    .filter((course) => {
      const matchesSearchQuery = course.course_name
        .toLowerCase()
        .includes(searchQuery.value.toLowerCase())
      const matchesCountry =
        !selectedCountry.value || course.country_id === selectedCountry.value.id
      const matchesState =
        !selectedState.value || course.state_id === selectedState.value.id
      return matchesSearchQuery && matchesCountry && matchesState
    })
    .sort((a, b) => {
      if (selectedSort.value) {
        const sortField = `${selectedSort.value.value.split('-')[0]}-${
          selectedSort.value.value.split('-')[1]
        }`
        const multiplier =
          selectedSort.value.value.split('-')[2] === 'asc' ? 1 : -1
        switch (sortField) {
          case 'qs-ranking': {
            const qsRankingA = Number(a.qs_ranking)
            const qsRankingB = Number(b.qs_ranking)
            return (qsRankingA - qsRankingB) * multiplier
          }
          case 'the-ranking': {
            const theRankingA = Number(a.the_ranking)
            const theRankingB = Number(b.the_ranking)
            return (theRankingA - theRankingB) * multiplier
          }
          case 'tuition-fee': {
            const tuitionFeeA = Number(a.tution_fee)
            const tuitionFeeB = Number(b.tution_fee)
            return (tuitionFeeA - tuitionFeeB) * multiplier
          }
          case 'living-fee': {
            const livingFeeA = Number(a.living_fee)
            const livingFeeB = Number(b.living_fee)
            return (livingFeeA - livingFeeB) * multiplier
          }
          case 'other-fee': {
            const otherFeeA = Number(a.other_fee)
            const otherFeeB = Number(b.other_fee)
            return (otherFeeA - otherFeeB) * multiplier
          }

          case 'app-fee': {
            const appFeeA = Number(a.app_fee)
            const appFeeB = Number(b.app_fee)
            return (appFeeA - appFeeB) * multiplier
          }
          case 'private-undefined':
          case 'public-undefined': {
            const isPrivate = sortField === 'private-undefined'
            const typeA = a.type.toLowerCase()
            const typeB = b.type.toLowerCase()

            if (typeA === typeB) return 0 // If both types are the same, return 0
            return (
              (typeA === (!isPrivate ? 'private' : 'public') ? -1 : 1) *
              multiplier
            )
          }
          case 'private-undefined':
          case 'public-undefined': {
            const isPrivate = sortField === 'private-undefined'
            const typeA = a.type.toLowerCase()
            const typeB = b.type.toLowerCase()

            if (typeA === typeB) return 0 // If both types are the same, return 0
            return (
              (typeA === (!isPrivate ? 'private' : 'public') ? -1 : 1) *
              multiplier
            )
          }
          case 'scholarship-undefined':
          case 'no-scholarship': {
            const hasScholarship = sortField === 'scholarship-undefined'
            const scholarshipA = a.scholarship.toLowerCase()
            const scholarshipB = b.scholarship.toLowerCase()

            if (scholarshipA === scholarshipB) return 0 // If both have the same scholarship status, return 0
            return (
              (scholarshipA === (!hasScholarship ? 'yes' : 'no') ? -1 : 1) *
              multiplier
            )
          }
          default:
            return 0
        }
      }
      return 0
    })
})
const paginatedCourse = computed(() => {
  const start = (currentPageCourse.value - 1) * itemsPerPage
  return filteredCourses.value.slice(start, start + itemsPerPage)
})

const onPageChanged = (page) => {
  currentPage.value = page
}
const onCoursePageChanged = (page) => {
  currentPageCourse.value = page
}

watch([searchQuery, selectedCountry, selectedState, selectedSort], () => {
  currentPage.value = 1
})
</script>
