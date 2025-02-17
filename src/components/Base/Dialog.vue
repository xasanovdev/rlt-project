<template>
      <Transition name="from-right">
        <div class="dialog" v-if="visible">
            <slot />
        </div>
      </Transition>
</template>

<script lang="ts" setup>

interface Props {
    visible: boolean
}

defineProps<Props>();

</script>

<style scoped lang="scss">
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
    z-index: 10;
}
</style>