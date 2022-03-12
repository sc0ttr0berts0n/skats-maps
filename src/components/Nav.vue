<script setup lang="ts">
import { ref } from 'vue';

const menuOpen = ref(false);
</script>

<template>
    <header class="header">
        <div class="logo-wrapper">
            <img class="icon" src="../assets/steamicon.jpg" alt="" />
            <h1>Skat&rsquo;s Maps</h1>
        </div>
        <div class="menu">
            <div class="menu-toggle" @click="menuOpen = !menuOpen">Maps</div>
            <nav class="nav" v-if="menuOpen">
                <div class="nav-item">Customs</div>
                <div class="nav-item">Factory</div>
                <div class="nav-item">Interchange</div>
                <div class="nav-item">Lighthouse</div>
                <div class="nav-item">Shoreline</div>
                <div class="nav-item">Woods</div>
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
}
.menu-toggle {
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
    &:hover {
        box-shadow: 0 0 1px 1px rgba(black, 0.8) inset,
            0 0 2rem rgba(rgb(255, 123, 0), 0.25);

        // background-image: linear-gradient(
        //     rgb(219, 216, 174),
        //     rgb(128, 122, 89)
        // );
    }
    &::after {
        display: inline-block;
        content: '▼';
        font-size: 0.75rem;
        transform: scaleY(0.86) translateY(-0.125rem);
    }
}
.nav {
    position: absolute;
    background-image: linear-gradient(
        rgba(34, 34, 30, 0.8),
        rgba(7, 6, 3, 0.8)
    );
    border-radius: 0.25rem;
    box-shadow: 0 0 1px 1px rgba(white, 0.5) inset,
        0 0 2rem rgba(rgb(255, 123, 0), 0.25);
    color: rgba(209, 195, 137, 0.75);
    text-shadow: rgba(34, 34, 30, 0.8) 1px 1px;
    margin-top: 0.375rem;
    padding: 0.75rem 0.75rem;
}

.nav-item {
    display: flex;
    justify-content: center;
    padding: 0.125rem 0;
    width: 100%;
    cursor: pointer;
    &:hover {
        text-decoration: underline;
    }
}
</style>
