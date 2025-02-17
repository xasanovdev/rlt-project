<script setup lang="ts">
import ThemeToggle from "../Common/ThemeToggle.vue";
</script>

<template>
    <header class="header">
        <div class="header__content">
            <h1>RLT Project</h1>

            <ThemeToggle />
        </div>
    </header>
</template>

<style scoped lang="scss">
.header {
    &__content {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
}
</style>
