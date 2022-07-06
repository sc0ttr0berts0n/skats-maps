<script setup lang="ts">
import { ref } from 'vue';

const props = defineProps({ items: Object });

const menuOpen = ref(true);

let menuTimeout: number | undefined = undefined;
const menuDelay = 400;

const delayedMenuClose = () => {
    menuTimeout = setTimeout(() => {
        menuOpen.value = false;
    }, menuDelay);
};

const cancelDelayedMenuClose = () => {
    clearTimeout(menuTimeout);
};
</script>

<template>
    <header class="header">
        <div class="logo-wrapper">
            <img class="icon" src="../assets/steamicon.jpg" alt="" />
            <h1>Skat&rsquo;s Maps</h1>
        </div>
        <div class="menu">
            <div class="button menu-toggle" @click="menuOpen = !menuOpen">
                Maps
            </div>
            <a
                href="https://tarkov.dev/ammo/"
                target="_blank"
                rel="nofollower"
                class="button"
                >Ammo</a
            >
            <nav
                class="nav"
                v-if="menuOpen"
                @mouseleave="delayedMenuClose"
                @mouseenter="cancelDelayedMenuClose"
            >
                <a
                    v-for="map in props.items"
                    class="nav-item"
                    :href="map.route"
                    >{{ map.name }}</a
                >
            </nav>
        </div>
    </header>
</template>

<style scoped lang="scss">
.header {
    height: calc(100% - 1rem);
    margin: 1rem 0;
    display: flex;
    align-items: center;
}

.logo-wrapper {
    display: flex;
    align-items: center;
    height: 100%;
}
.icon {
    height: 100%;
    width: auto;
    border-radius: 50%;
    margin-right: 1rem;
}

.menu {
    position: relative;
    padding-left: 1rem;
    font-size: 1.25rem;
    font-weight: bold;
    display: flex;
}
.button {
    background-image: linear-gradient(rgb(248, 245, 199), rgb(153, 146, 106));
    border-radius: 0.25rem;
    box-shadow: 0 0 1px 1px rgba(white, 0.5) inset,
        0 0 2rem rgba(rgb(255, 123, 0), 0.25);
    color: rgba(14, 13, 9, 0.75);
    text-shadow: white 1px 1px;
    padding: 0.75rem 1.5rem;
    user-select: none;
    cursor: pointer;
    transition: 200ms;
    margin-left: 1rem;
    text-decoration: none;
    &:first-child {
        margin-left: 0;
    }
    &:hover {
        box-shadow: 0 0 1px 1px rgba(black, 0.8) inset,
            0 0 2rem rgba(rgb(255, 123, 0), 0.25);

        // background-image: linear-gradient(
        //     rgb(219, 216, 174),
        //     rgb(128, 122, 89)
        // );
    }
    &.menu-toggle {
        white-space: nowrap;
    }
    &.menu-toggle::after {
        display: inline-block;
        content: '▼';
        font-size: 0.75rem;
        transform: scaleY(0.86) translateY(-0.125rem);
    }
}
.nav {
    position: absolute;
    background-image: linear-gradient(
        rgba(34, 34, 30, 0.9),
        rgba(7, 6, 3, 0.9)
    );
    border-radius: 0.25rem;
    box-shadow: 0 0 1px 1px rgba(white, 0.5) inset,
        0 0 2rem rgba(rgb(255, 123, 0), 0.25);
    text-shadow: rgba(34, 34, 30, 0.8) 1px 1px;
    margin-top: 0.375rem;
    padding: 0.75rem 0.75rem;
    z-index: 1;
}

.nav-item {
    display: flex;
    justify-content: center;
    padding: 0.125rem 0;
    width: 100%;
    cursor: pointer;
    color: rgba(209, 195, 137, 0.75);
    text-decoration: none;
    &:hover {
        color: #ffffff;
        text-decoration: underline;
    }
}
</style>
