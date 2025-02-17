<template>
    <aside class="sidebar wrapper">    
        <div class="sidebar__image">
            <img src="/sidebar-image.png" alt="sidebar-image">
        </div>

        <div class="sidebar__info">
            <div class="shimmer sidebar__info--title"></div>
            <div class="sidebar__info--subtitle">
                <div class="shimmer"></div>
                <div class="shimmer"></div>
                <div class="shimmer"></div>
                <div class="shimmer"></div>
                <div class="shimmer"></div>
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
    gap: 20px;
    max-width: 236px;

    @media (max-width: 820px) {
      max-width: 100%;
      flex-direction: row;
    }

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

        .shimmer:nth-child(2) {
            width: 90%;
        }

        .shimmer:nth-child(3) {
            width:70%;
        }
        .shimmer:nth-child(4) {
            width: 65%;
        }
        .shimmer:nth-child(5) {
            width: 64%;
        }

        .shimmer {
            width: 70%;
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
