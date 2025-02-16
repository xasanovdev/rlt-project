<template>
  <button class="custom-button" :class="[buttonClass, type]">
    <slot></slot>
  </button>
</template>

<script lang="ts" setup>
import { defineComponent } from 'vue';


interface Props {
    buttonClass?: string;
    type?: 'primary' | 'secondary' | 'danger';
}

defineProps<Props>();

defineComponent({
    name: 'CustomButton'
})

</script>

<style lang="scss" scoped>
.custom-button {
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s;

  &.primary {
    background-color: #007bff;

    &:hover {
      background-color: #0056b3;
    }
  }

  &.secondary {
    background-color: #6c757d;

    &:hover {
      background-color: #5a6268;
    }
  }

  &.danger {
    background-color: #dc3545;

    &:hover {
      background-color: #c82333;
    }
  }
}
</style>
