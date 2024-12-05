<template>
  <div class="rounded-lg border shadow-lg flex flex-col justify-between">
    <img
      :src="`https://unihunt.freeapi.in/assets/uploads/${university.university_image}`"
      alt="University Image"
      class="w-full h-56 object-scale-down rounded-t-lg border-b-2"
    />
    <div class="px-6 py-4">
      <h2 class="font-bold text-xl">{{ university.university_name }}</h2>
      <p class="text-gray-700 text-base">
        {{ university.state_name }}, {{ university.country_name }}
      </p>
      <div class="grid grid-cols-2 gap-4 mt-4">
        <p class="text-sm font-medium text-gray-500 capitalize">
          Type: {{ university.type }}
        </p>
        <p class="text-sm font-medium text-gray-500 capitalize">
          Scholarship: {{ university.scholarship }}
        </p>
        <p class="text-sm font-medium text-gray-500">
          QS Ranking: {{ university.qs_ranking }}
        </p>
        <p class="text-sm font-medium text-gray-500">
          The Ranking: {{ university.the_ranking }}
        </p>
        <p class="text-sm font-medium text-gray-500">
          Tuition Fee: {{ currencySymbol }}{{ university.tution_fee }}
        </p>
        <p class="text-sm font-medium text-gray-500">
          Application Fee: {{ currencySymbol }}{{ university.app_fee }}
        </p>
        <p class="text-sm font-medium text-gray-500">
          Living Fee: {{ currencySymbol }}{{ university.living_fee }}
        </p>
        <p class="text-sm font-medium text-gray-500">
          Others Fee: {{ currencySymbol }}{{ university.other_fee }}
        </p>
        <p class="text-sm font-medium text-gray-500">
          Views: {{ university.university_views }}
        </p>
      </div>
    </div>
    <div class="px-6 py-4 text-right">
      <a
        :href="university.university_website"
        class="text-blue-500 hover:text-blue-700"
        >Visit Website</a
      >
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  university: {
    type: Object,
    required: true,
  },
  currencySymbol: {
    type: String,
    default: '$',
  },
})
</script>

<style scoped>
/* You can customize styles as needed */
</style>
