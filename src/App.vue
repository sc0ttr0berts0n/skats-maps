<script setup lang="ts">
import { reactive } from 'vue';
import Nav from './components/Nav.vue';
import Map from './components/Map.vue';
import maplist from './assets/maps.json';

export interface IMap {
    name: string;
    varient: string;
    url: string;
    width: number;
    height: number;
}

const maps: IMap[] = reactive(maplist);
</script>

<template>
    <Nav />
    <Map :map="maps[0]" />
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
