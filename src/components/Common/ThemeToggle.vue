<script setup lang="ts">
import { Moon, Sun } from "lucide-vue-next";
import { onMounted, ref } from "vue";

const theme = ref(localStorage.getItem("theme") || "light");

const toggleTheme = () => {
    theme.value = theme.value === "light" ? "dark" : "light";
    document.documentElement.setAttribute("data-theme", theme.value);
    localStorage.setItem("theme", theme.value);
};

onMounted(() => {
    document.documentElement.setAttribute("data-theme", theme.value);
});
</script>

<template>
    <button class="theme-toggle" @click="toggleTheme">
        <Sun v-if="theme === 'dark'" />
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
