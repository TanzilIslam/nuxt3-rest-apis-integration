<template>
  <div
    class="bg-white border rounded-lg shadow-md p-4 flex flex-col justify-between"
  >
    <div class="flex items-center gap-4 mb-4">
      <img
        :src="
          course?.university_image
            ? `https://unihunt.freeapi.in/assets/uploads/${course?.university_image}`
            : '~/public/bg-cover.png'
        "
        alt="University Image"
        class="h-24 w-16 rounded-full object-scale-down"
      />
      <div>
        <h2 class="text-xl font-semibold text-gray-800">
          {{ course.course_name }}
        </h2>
        <NuxtLink
          :to="course.university_website"
          class="text-sm font-bold mt-4 block hover:underline"
        >
          {{ course.university_name }}
        </NuxtLink>
        <p class="text-sm">
          {{ course.state_name }}, {{ course.country_name }}
        </p>
      </div>
    </div>

    <p class="text-gray-700 mb-4 line-clamp-3">{{ course.course_notes }}</p>

    <div class="grid grid-cols-2 gap-4 mb-4 text-sm">
      <div>
        <strong class="text-gray-800">Qualification:</strong>
        {{ course.qualification }}
      </div>
      <div>
        <strong class="text-gray-800">Credits:</strong> {{ course.credits }}
      </div>
      <div>
        <strong class="text-gray-800">Tuition Fee:</strong>
        {{ course.currency_symbol }}{{ course.tution_fee }}
      </div>
      <div>
        <strong class="text-gray-800">Minimum Percentage:</strong>
        {{ course.min_percentage }}
      </div>
      <div>
        <strong class="text-gray-800">Required Toefl Score:</strong>
        {{ course.toefl }}
      </div>
      <div>
        <strong class="text-gray-800">Required Ielts Score:</strong>
        {{ course.ielts }}
      </div>
    </div>

    <div class="text-sm flex justify-between items-center">
      <NuxtLink :to="course.course_url" class="text-blue-600 hover:underline">
        View Course Details
      </NuxtLink>
      <NuxtLink
        target="_blank"
        :to="course?.app_url"
        class="bg-blue-600 text-white px-4 py-1.5 rounded-lg hover:bg-blue-700 focus:outline-none"
        >Apply
      </NuxtLink>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  course: {
    type: Object,
    required: true,
  },
})
</script>

<style scoped>
/* Scoped styles for additional customization if necessary */
</style>
