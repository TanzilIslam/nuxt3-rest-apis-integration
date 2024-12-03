<template>
  <button
    @click="toggleTheme"
    class="focus:outline-none flex items-end"
    aria-label="Toggle theme"
  >
    <BaseIcon :name="isDarkMode ? 'sun' : 'moon'" />
  </button>
</template>

<script setup>
import BaseIcon from "~/components/BaseIcon.vue";
const isDarkMode = ref(false);

const toggleTheme = () => {
  isDarkMode.value = !isDarkMode.value;
  document.documentElement.classList.toggle("dark");
};
</script>
