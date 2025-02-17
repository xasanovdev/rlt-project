<!-- Dialog.vue -->
<script setup lang="ts">
import { X } from 'lucide-vue-next';
import { ref, watchEffect } from 'vue';
import CustomButton from '../Base/Button.vue';
import Dialog from '../Base/Dialog.vue';
import Input from '../Base/Input.vue';

interface Props {
  visible: boolean;
  item: {
    index: number;
    isActive: boolean;
    count: number;
    color: string;
  };
  colors: string[];
}

const props = defineProps<Props>();

const emit = defineEmits(['update:visible', 'update:item', 'confirm', 'delete']);

const localItem = ref({ ...props.item });

watchEffect(() => {
  if (props.visible) {
    localItem.value = { ...props.item };
  }
});

const handleConfirm = () => {
  emit('confirm', { ...localItem.value, isActive: true });
  emit('update:visible', false);
};

const handleDelete = () => {
  emit('delete', localItem.value.index);
  emit('update:visible', false);
};

const handleClose = () => {
  emit('update:visible', false);
};

const handleColorChange = (color: string) => {
  localItem.value.color = color;
  emit('update:item', { ...localItem.value });
};
</script>

<template>
    <Dialog :visible="visible" @close="handleClose">
        <div class="dialog__content">
            <div class="dialog__content-image" :style="{ backgroundColor: localItem.color }"></div>

            <form v-if="!localItem.isActive" class="dialog__content-colors" @submit.prevent>
                <label v-for="color in colors" :key="color" class="radio-label">
                <input
                    type="radio"
                    v-model="localItem.color"
                    :value="color"
                    class="input-radio"
                    @change="handleColorChange(color)"
                    />
                    <button
                    class="radio-color"
                    :class="{ 'active': localItem.color === color }"
                    :style="{ backgroundColor: color }"
                    ></button>
                </label>
            </form>

            <hr class="dialog__content-line" />

            <div class="dialog__content-info">
                <div class="dialog__content-info--title shimmer"></div>
                <div class="dialog__content-info--subtitle">
                    <div v-for="i in 4" :key="i" class="shimmer"></div>
                </div>
                <div class="dialog__content-info--footer shimmer"></div>
            </div>

            <template v-if="localItem.isActive">
                <hr class="dialog__content-line" />
                <CustomButton class="delete__button" @click="handleDelete" type="danger" size="medium">
                    Удалить предмет
                </CustomButton>
            </template>
        </div>

        <div v-if="!localItem.isActive" class="dialog__widget">
            <Input
                v-model="localItem.count"
                class="dialog__widget-input"
                type="number"
                :max="100"
                :min="1"
                placeholder="Введите количество"
                />

                <div class="dialog__widget-buttons">
                    <CustomButton @click="handleClose" type="primary" size="medium">
                        Отмена
                    </CustomButton>

                    <CustomButton @click="handleConfirm" type="danger" size="medium">
                        Подтвердить
                    </CustomButton>
                </div>
        </div>

        <X class="dashboard__chat-icon" @click="handleClose" />
    </Dialog>
</template>

<style scoped lang="scss">
.dialog {
  &__widget {
    padding: 20px;
    background: var(--dark-gray);
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
    border-top: 1px solid var(--secondary-color);
    backdrop-filter: blur(1rem);

    &-buttons {
      display: flex;
      gap: 10px;
    }

    &-input {
      width: 100%;
    }
  }

  &__content {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
    width: 100%;
    height: 100%;
    padding-top: 55px;
    overflow-y: auto;

    &-colors {
      display: flex;
      align-items: center;
      gap: 20px;
      width: 100%;
      margin-top: 2rem;

      .radio-label {
        width: 100%;
        position: relative;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
      }

      .input-radio {
        width: 100%;
        height: 100%;
        position: absolute;
        cursor: pointer;
        opacity: 0;
      }

      .radio-color {
        width: 20px;
        height: 20px;
        border-radius: 50%;

        &.active {
          outline: 4px solid var(--secondary-color) !important;
        }
      }
    }

    .delete__button {
      width: 100%;
    }

    &-line {
      width: 100%;
      height: 1px;
      background-color: var(--secondary-color);
      margin-top: 1rem;
      margin-bottom: 1rem;
    }

    &-info {
      margin-top: 20px;
      width: 100%;
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 20px;

      &--title {
        max-width: 190px;
        width: 100%;
        height: 26px;
        border-radius: 8px;
      }

      &--subtitle {
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
        gap: 1rem;
        width: 100%;

        .shimmer {
          width: 88%;
          height: 10px;
          border-radius: 8px;
        }
      }

      &--footer {
        width: 80px;
        height: 10px;
        border-radius: 8px;
      }
    }

    &-image {
      width: 115px;
      height: 115px;
      position: relative;
      background-color: var(--secondary-color);
      z-index: 2;
      flex-shrink: 0;

      &::before {
        content: '';
        position: absolute;
        top: -14px;
        left: 14px;
        width: 100%;
        height: 100%;
        backdrop-filter: blur(1rem);
      }
    }
  }
}

.dashboard__chat-icon {
  position: absolute;
  right: 10px;
  top: 10px;
  border-radius: 50%;

  &:hover {
    color: var(--red-color);
    cursor: pointer;
    transition: all 0.3s;
  }
}
</style>