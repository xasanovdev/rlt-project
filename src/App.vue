<script setup lang="ts">
import Header from "./components/Layout/Header.vue";
import Main from "./components/Layout/Main.vue";
</script>

<template>
    <div class="container app_content">
        <Header />

        <Main />
    </div>
</template>

<style scoped lang="scss">
.app_content {
    margin-top: 12px;
}
</style>
