<script setup lang="ts">
import { reactive, ref, computed } from 'vue';
import Nav from './components/Nav.vue';
import Map from './components/Map.vue';
import maplist from './assets/maps.json';

export interface IMap {
    name: string;
    route: string;
    varient: string;
    url: string;
    width: number;
    height: number;
}

const maps: IMap[] = reactive(maplist);

const baseTitle = 'Skat’s Maps';

const currentPath = ref(window.location.hash);

window.addEventListener('hashchange', () => {
    const rawSubtitle = window.location.hash.split('#').join('');
    const subtitle =
        rawSubtitle[0].toUpperCase() + rawSubtitle.slice(1).toLowerCase();
    currentPath.value = window.location.hash;
    document.title = `${subtitle} | ${baseTitle}`;
});

const currentMap = computed(() => {
    // return maps[1];
    const target = maps.find((map) => {
        return map.route === currentPath.value;
    });

    return target ?? maps[0];
});
</script>

<template>
    <Nav :items="maps" />
    <Map :map="currentMap" :ref="currentPath" />
</template>

<style lang="scss">
* {
    margin: 0;
    padding: 0;
}
body {
    background-color: #2c3e50;
}
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #e8f1fa;
    display: grid;
    grid-template-rows: 4rem 1fr;
    gap: 1rem;
    margin: 0 1rem 1rem;
    width: calc(100% - 2rem);
    height: calc(100vh - 2rem);
}
</style>
