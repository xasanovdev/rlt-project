<script setup lang="ts">
import { X } from 'lucide-vue-next';
import { ref, onMounted, watch } from 'vue';
import Sidebar from './Sidebar.vue';
import DashboardItemDialog from '../Common/DashboardItemDialog.vue';

interface Item {
    index: number;
    isActive: boolean;
    count: number;
    color: string
    isBeingDragged: boolean
}

const colors = ref(['#7FAA65', '#AA9765', '#656CAA']);

const itemsList = ref<Array<Item>>([]);

const loadItemsFromStorage = () => {
    const savedItems = localStorage.getItem('itemsList');

    if (savedItems) {
        itemsList.value = JSON.parse(savedItems);
    } else {
        itemsList.value = Array.from({ length: 25 }, (_, index) => ({
            index: index + 1,
            isActive: false,
            count: 0,
            color: colors.value[index % colors.value.length],
            isBeingDragged: false
        }));
    }
};

watch(() => itemsList.value, (newList) => {
    localStorage.setItem('itemsList', JSON.stringify(newList));
}, { deep: true });

onMounted(() => {
  loadItemsFromStorage()
});

const dialogVisible = ref(false);

const selectedItem = ref<Item>({
    index: 0,
    isActive: false,
    count: 0,
    color: colors.value[0],
    isBeingDragged: false
});

const handleClick = (item: Item) => {
    selectedItem.value = { ...item };
    dialogVisible.value = true;
};

const handleConfirm = (updatedItem: Item) => {
    itemsList.value[updatedItem.index - 1] = updatedItem;
};

const handleDelete = (index: number) => {
    const idx = itemsList.value.findIndex(item => item.index === index);
    itemsList.value[idx] = { ...itemsList.value[idx], isActive: false, count: 0 };
};

const handleDragStart = (item: Item) => {
  dialogVisible.value = false;

  selectedItem.value = { ...item };
}

const handleDrop = (item: Item) => {
    if (item) {
        itemsList.value[item.index - 1] = {
            color: selectedItem.value.color,
            count: selectedItem.value.count,
            isActive: true,
            index: item.index,
            isBeingDragged: false
        };

        selectedItem.value = { ...selectedItem.value, isActive: false, count: 0 };

        itemsList.value[selectedItem.value.index - 1] = { ...itemsList.value[selectedItem.value.index - 1], isActive: false, count: 0 }
    }
};
</script>

<template>
  <main class="main_content">
    <div class="dashboard">
      <Sidebar />

      <div class="dashboard__main wrapper">
        <button
          class="dashboard__main-item"
          v-for="item in itemsList"
          @click="handleClick(item)"
          draggable="true"
          @dragover.prevent
          @drop="handleDrop(item)"
          @dragstart="handleDragStart(item)"
          @dragenter="item.isBeingDragged = true"
          @dragleave="item.isBeingDragged = false"
          :key="item.index"
          :aria-label="`Перетащите предмет ${item.index}`"
          :data-item-index="item.index"
          type="button"
          :class="{ 'dashboard__main-item--dragging': item.isBeingDragged }"
        >
          <div v-if="item.isActive" class="dashboard__main-item-active">
            <div class="dashboard__main-item--image" :style="{ backgroundColor: item.color }"></div>
            <span class="dashboard__main-item--count">{{ item.count }}</span>
          </div>
        </button>

        <DashboardItemDialog
          v-model:visible="dialogVisible"
          v-model:item="selectedItem"
          :colors="colors"
          @confirm="handleConfirm"
          @delete="handleDelete"
        />
      </div>
    </div>

    <div class="dashboard__chat wrapper">
      <div class="dashboard__chat-item shimmer"></div>
      <X class="dashboard__chat-icon" />
    </div>
  </main>
</template>

<style scoped lang="scss">
.main_content {
  margin-top: 12px;
  position: relative;
}

.dashboard {
  display: flex;
  gap: 24px;

  @media (max-width: 820px) {
    flex-direction: column;
  }

  &__main {
    display: grid;
    padding: 0px !important;
    width: 100% !important;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(5, 1fr);
    overflow: hidden;
    max-width: 525px;
    position: relative;

    @media (max-width: 820px) {
      max-width: 100%;
    }

    @media (max-width: 480px) {
      grid-template-columns: repeat(3, 1fr);
    }

    &-item {
      position: relative;
      height: 100px;
      border: 1px solid var(--secondary-color);
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;  

      &--dragging {
        border: 4px solid var(--secondary-color);
      }

      &--active {
        width: 100%;
        height:100%;
      }

      &:hover {
        background-color: var(--gray);
      }

      &--image {
        width: 48px;
        height: 48px;
        position: relative;
        background-color: var(--secondary-color);
        z-index: 2;
        flex-shrink: 0;

        &::before {
          content: '';
          position: absolute;
          top: -6px;
          left: 6px;
          z-index: 1;
          width: 100%;
          height: 100%;
          backdrop-filter: blur(1rem);
        }
      }

      &--count {
        position: absolute;
        bottom: 0px;
        right: 0px;
        font-size: 10px;
        color: var(--secondary-color);
        padding: 2px 4px;
        border: 1px solid var(--secondary-color);
        border-top-left-radius: 6px;
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
    margin-top: 24px;

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