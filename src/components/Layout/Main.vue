<script setup lang="ts">
import CustomButton from '../Base/Button.vue';
import { X } from 'lucide-vue-next';
import { ref } from 'vue';
import Sidebar from './Sidebar.vue';

interface Item {
    index: number;
    isActive: boolean;
    count: number;
    color: string;
}

const colors = ref(['#7FAA65', '#AA9765', '#656CAA']);

const itemsList = ref<Array<Item>>([]);

itemsList.value = Array.from({ length: 25 }, (_, index) => ({
    index: index + 1,
    isActive: false,
    count: 0,
    color: colors.value[index % colors.value.length]
}));


const dialogVisible = ref(false);

const selectedItem = ref<Item>({
    index: 0,
    isActive: false,
    count: 0,
    color: colors.value[0]
});

const handleClick = (item: Item) => {
    dialogVisible.value = true;

    selectedItem.value = { ...item };
};

const handleConfirm = () => {
    dialogVisible.value = false;

    itemsList.value[selectedItem.value.index - 1] = { ...selectedItem.value, isActive: true };
};

const deleteSelectedActiveItem = () => {
    const index = itemsList.value.findIndex(item => item.index === selectedItem.value.index);

    itemsList.value[index] = { ...itemsList.value[index], isActive: false, count: 0 };

    dialogVisible.value = false;
};

</script>

<template>
  <main class="main_content">
    <div class="dashboard">
        <Sidebar />

        <div class="dashboard__main wrapper">
            <button class="dashboard__main-item" v-for="(item, index) in itemsList" 
                @click="handleClick(item)" :key="index" type="button">
                <span class="dashboard__main-item-index">
                </span>
            </button>


            <Transition name="from-right">
                <div class="dialog" v-if="dialogVisible">
                    <div class="dialog__content">
                        <div class="dialog__content-image"
                            :style="{ backgroundColor: selectedItem.color }"
                        >
                        </div>

                        <form class="dialog__content-colors" @submit.prevent>
                          <label v-for="color in colors" :key="color" class="radio-label">
                            <input
                              type="radio"
                              v-model="selectedItem.color"
                              :value="color"
                              class="input-radio"
                            />

                            <button class="radio-color" :class="{ 'active': selectedItem.color === color }" :style="{ backgroundColor: color }"></button>
                          </label>
                        </form>

                        <hr class="dialog__content-line">

                        <div class="dialog__content-info">
                            <div class="dialog__content-info--title shimmer"></div>
                            <div class="dialog__content-info--subtitle">
                                <div v-for="i in 4" class="shimmer"></div>
                            </div>
                            <div class="dialog__content-info--footer shimmer"></div>
                        </div>

                        <template v-if="selectedItem.isActive">
                            <hr class="dialog__content-line">

                            <CustomButton class="delete__button" @click="deleteSelectedActiveItem" type="danger" size="medium">
                                Удалить предмет
                            </CustomButton>
                        </template>
                    </div>

                    <div v-if="!selectedItem.isActive" class="dialog__widget">
                        <input v-model="selectedItem.count" class="dialog__widget-input" type="number" max="100" min="1" placeholder="Введите количество"/>

                        <div class="dialog__widget-buttons">
                            <CustomButton @click="dialogVisible = false" type="primary" size="medium">
                                Отмена
                            </CustomButton>

                            <CustomButton @click="handleConfirm" type="danger" size="medium">
                                Подтвердить
                            </CustomButton>
                        </div>
                    </div>


                    <X class="dashboard__chat-icon" @click="dialogVisible = false" />
                </div>
            </Transition>
        </div>

        <div class="dashboard__chat wrapper">
            <div class="dashboard__chat-item shimmer"></div>

            <X class="dashboard__chat-icon" />
        </div>
    </div>
  </main>
</template>

<style scoped lang="scss">
.main_content {
    margin-top: 12px;
    position: relative;

    .dialog {
        position: absolute;
        backdrop-filter: blur(1rem);
        top: 0;
        right: 0;
        width: 250px;
        height: 100%;
        display: flex;
        flex-direction: column;
        border-left: 1px solid var(--secondary-color);

        &__widget {
            padding: 20px;
            background: var(--dark-gray);
            width: 100%;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
            border-top: 1px solid var(--secondary-color);

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
                  width:100%;
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
                    z-index: 1;
                    width: 100%;
                    height: 100%;
                    backdrop-filter: blur(1rem);
                }
            }
        }
    }
}

.dashboard {
  display: flex;
  flex-wrap: wrap;
  gap: 24px;

  &__main {
    display: grid;
    padding: 0px !important;
    width: 100% !important;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(5, 1fr);
    overflow: hidden;
    max-width: 525px;
    position: relative;

    &-item {
      position: relative;
      height: 100px;
      border: 1px solid var(--secondary-color);
      cursor: pointer;

      &:hover {
        background-color: var(--gray);
      }
    }

    &-icon {
      position: absolute;
      top: 20px;
      left: 50px;
      border-radius: 50%;
      padding: 5px;
    }
  }

  &__chat {
    width: 100%;
    position: relative;

    &-item {
      width: 100%;
      max-width: 699px;
      height: 36px;
      border-radius: 8px;
      position: relative;
    }

    &-icon {
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
  }
}


</style>