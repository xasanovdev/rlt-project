<script setup lang="ts">
import CustomButton from '../Base/Button.vue';
import { X } from 'lucide-vue-next';
import { ref } from 'vue';

const itemsList = ref<Array<{ index: number; isActive: boolean; count: number }>>([]);

itemsList.value = Array.from({ length: 25 }, (_, index) => ({
    index: index + 1,
    isActive: false,
    count: 0,
    color: ''
}));

const colors = ref(['#7FAA65', '#AA9765', '#656CAA']);

const dialogVisible = ref(false);

const selectedItem = ref<{ index: number; isActive: boolean; count: number; color: string }>({
    index: 0,
    isActive: false,
    count: 0,
    color: ''
});

const handleClick = (item: { index: number; isActive: boolean; count: number; color: string }) => {
    dialogVisible.value = true;
    selectedItem.value = item;
};

</script>

<template>
  <main class="main_content">
    <div class="dashboard">
        <div class="dashboard__sidebar wrapper">    
            <div class="dashboard__sidebar-image">
                <img src="/sidebar-image.png" alt="sidebar-image">
            </div>

            <div class="dashboard__sidebar-info">
                <div class="shimmer dashboard__sidebar-info--title"></div>
                <div class="dashboard__sidebar-info--subtitle">
                    <div v-for="i in 4" class="shimmer"></div>
                </div>
                <div class="shimmer dashboard__sidebar-info--footer"></div>
            </div>
        </div>

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
                            :style="{ backgroundColor: selectedItem?.color }"
                        >
                        </div>

                        <hr class="dialog__content-line">

                        <div class="dialog__content-info">
                            <div class="dialog__content-info--title shimmer"></div>
                            <div class="dialog__content-info--subtitle">
                                <div v-for="i in 4" class="shimmer"></div>
                            </div>
                            <div class="dialog__content-info--footer shimmer"></div>
                        </div>

                        <hr class="dialog__content-line">
                        
                        <CustomButton class="delete__button" @click="dialogVisible = false" type="danger">
                            Удалить предмет
                        </CustomButton>
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

        &__content {
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 1rem;
            width: 100%;
            height: 100%;
            padding-top: 55px;

            .delete__button {
                width: 100%;
            }

            &-line {
                width: 100%;
                height: 1px;
                background-color: var(--secondary-color);
                margin-top: 2rem;
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
  display: grid;
  grid-template-columns: 1fr 3fr;
  gap: 1rem;

  &__sidebar {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;

    &-image {
      width: 100%;
      border-radius: 8px;
      overflow: hidden;

      img {
        width: 100%;
      }
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
  }

  &__main {
    border-radius: 12px;
    display: grid;
    padding: 0px !important;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(5, 1fr);
    overflow: hidden;
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
    grid-column: span 2;
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