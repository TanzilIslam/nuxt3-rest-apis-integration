<template>
  <Icon :name="`heroicons:${name}`" :class="iconClass" />
</template>

<script setup>
const props = defineProps({
  name: {
    type: String,
    required: true,
  },
  iconClass: {
    type: String,
    default: "h-5 w-5",
  },
});
</script>
