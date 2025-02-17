<template>
    <div class="input-wrapper">
      <input
        :type="type"
        v-model="model"
        class="quantity-input"
        :placeholder="placeholder"
        :min="min"
        :max="max"
        :disabled="disabled"
        :required="required"
      />
    </div>
  </template>
  
<script setup lang="ts">
interface Props {
    modelValue: string | number;
    placeholder?: string;
    type?: string;
    min?: number;
    max?: number;
    disabled?: boolean;
    required?: boolean;
}
  
withDefaults(defineProps<Props>(), {
    placeholder: 'Введите количество',
    type: 'number',
    min: undefined,
    max: undefined, 
    disabled: false,
    required: false
});
  
const model = defineModel()

</script>

<style lang="scss" scoped>
.input-wrapper {
    width: 100%;
    max-width: 380px;
    
    .quantity-input {
      width: 100%;
      padding: 10px 11px;
      color: var(--text-color);
      font-size: 14px;
      background-color: var(--dark-gray);
      border: 1px solid var(--secondary-color);
      border-radius: 5px;
      outline: none;
      transition: all 0.3s ease;
      
      &::placeholder {
        color: #777;
        font-size: 14px;
      }
      
      &:focus {
        border-color: var(--primary-color);
        box-shadow: 0 0 0 2px rgba(74, 144, 226, 0.2);
      }
      
      &:disabled {
        opacity: 0.7;
        cursor: not-allowed;
      }
      
      &[type="number"]::-webkit-inner-spin-button,
      &[type="number"]::-webkit-outer-spin-button {
        -webkit-appearance: none;
        margin: 0;
      }
      
      &[type="number"] {
        -moz-appearance: textfield;
      }
    }
  }
  </style>