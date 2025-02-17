<script setup lang="ts">
import type { Ref } from "vue";
import Modal from "../Base/Modal.vue";
import Button from "../Base/Button.vue";

interface Props {
    title: string;
    subtitle: string;
}

interface Emits {
    (e: "confirm"): void;
    (e: "cancel"): void;
}

defineProps<Props>();

defineEmits<Emits>();

const showWarningModal = defineModel() as Ref<boolean>;
</script>

<template>
    <Modal v-model="showWarningModal" title="">
        <div class="warning-content">
            <h3 class="warning-titile">{{ title }}</h3>
            <p class="warning-message">{{ subtitle }}</p>
            <div class="warning-actions">
                <Button type="primary" @click="$emit('cancel')">Отмена</Button>
                <Button type="danger" @click="$emit('confirm')"
                    >Подтвердить</Button
                >
            </div>
        </div>
    </Modal>
</template>

<style scoped lang="scss">
.warning-content {
    text-align: center;
    padding: 10px;
}

.warning-message {
    margin-bottom: 24px;
    font-size: 16px;
    line-height: 1.5;
}

.warning-actions {
    display: flex;
    justify-content: center;
    gap: 16px;
}
</style>
