<template>
    <button class="custom-button" :class="[buttonClass, type, size]">
        <slot></slot>
    </button>
</template>

<script lang="ts" setup>
import { defineComponent } from "vue";

interface Props {
    buttonClass?: string;
    type?: "primary" | "secondary" | "danger";
    size?: "small" | "medium" | "large";
}

defineProps<Props>();

defineComponent({
    name: "CustomButton",
});
</script>

<style lang="scss" scoped>
.custom-button {
    padding: 10px 20px;
    border: none;
    border-radius: 8px;
    cursor: pointer;
    transition: background-color 0.3s;

    &.primary {
        background-color: white;
        color: black;

        &:hover {
            background-color: var(--primary-color);
        }
    }

    &.secondary {
        background-color: var(--secondary-color);

        &:hover {
            background-color: #5a6268;
        }
    }

    &.danger {
        background-color: var(--red-color);
        color: white;

        &:hover {
            background-color: #bf4551;
        }
    }

    &.small {
        padding: 5px 10px;
        font-size: 12px;
    }

    &.medium {
        padding: 8px 15px;
        font-size: 14px;
    }

    &.large {
        padding: 15px 30px;
        font-size: 16px;
    }
}
</style>
