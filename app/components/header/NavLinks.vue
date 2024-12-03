<template>
  <NuxtLink
    :to="link.to"
    class="text-white hover:text-gray-300 transition-colors flex items-center gap-1"
    :aria-label="link.title"
  >
    <BaseIcon :name="link.icon" />
    {{ link.title }}
  </NuxtLink>
</template>

<script setup>
import BaseIcon from "~/components/BaseIcon.vue";
const props = defineProps({
  link: {
    type: Object,
    required: true,
  },
});
</script>
