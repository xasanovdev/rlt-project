<script setup>
import { Moon, Sun } from 'lucide-vue-next';
import { ref, onMounted, computed } from 'vue';

const theme = ref(localStorage.getItem('theme') || 'light');

const toggleTheme = () => {
  theme.value = theme.value === 'light' ? 'dark' : 'light';
  document.documentElement.setAttribute('data-theme', theme.value);
  localStorage.setItem('theme', theme.value);
};

onMounted(() => {
  document.documentElement.setAttribute('data-theme', theme.value);
});
</script>

<template>
  <button class="theme-toggle" @click="toggleTheme">
    <Sun v-if="theme === 'light'" />
    <Moon v-else />
  </button>
</template>

<style scoped lang="scss">
.theme-toggle {
  padding: 0.5rem 1rem;
  cursor: pointer;
  border-radius: 5px;
}
</style>
