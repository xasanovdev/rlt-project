<template>
    <aside class="sidebar wrapper">    
        <div class="sidebar__image">
            <img src="/sidebar-image.png" alt="sidebar-image">
        </div>

        <div class="sidebar__info">
            <div class="shimmer sidebar__info--title"></div>
            <div class="sidebar__info--subtitle">
                <div v-for="i in 4" class="shimmer"></div>
            </div>
            <div class="shimmer sidebar__info--footer"></div>
        </div>
    </aside>
</template>

<style scoped lang="scss">
  .sidebar {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
    max-width: 236px;

    &__image {
      width: 100%;
      border-radius: 8px;
      overflow: hidden;

      img {
        width: 100%;
      }
    }

    &__info {
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
</style>
