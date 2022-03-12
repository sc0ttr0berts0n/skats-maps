<script setup lang="ts">
import { ref, Ref, onMounted } from 'vue';
import Panzoom, { PanzoomObject } from '@panzoom/panzoom';
import { IMap } from '../App.vue';

const props = defineProps({
    map: Object,
});

const mapWrapper = ref() as Ref<HTMLElement>;
const mapElement = ref() as Ref<HTMLElement>;
let panzoom: PanzoomObject;

onMounted(() => {
    const padding = 16;
    const bounds = mapWrapper.value.getBoundingClientRect();
    const map = mapElement.value.getBoundingClientRect();
    const widthRatio = (bounds.width - padding) / map.width;
    const heightRatio = (bounds.height - padding) / map.height;
    const startScale = Math.min(widthRatio, heightRatio);
    const shortAxis = widthRatio > heightRatio ? 'x' : 'y';
    const halfMapWidth = map.width / 2;
    const halfMapWidthScaled = halfMapWidth * startScale;
    const halfMapHeight = map.height / 2;
    const halfMapHeightScaled = halfMapHeight * startScale;
    const xOff =
        shortAxis === 'x' ? bounds.width / startScale / 2 - map.width / 2 : 0;
    const yOff =
        shortAxis === 'y' ? bounds.height / startScale / 2 - map.height / 2 : 0;

    const startX =
        -(halfMapWidth - halfMapWidthScaled) / startScale + xOff + padding;
    const startY =
        -(halfMapHeight - halfMapHeightScaled) / startScale + yOff + padding;

    panzoom = Panzoom(mapElement.value!, {
        startScale,
        maxScale: 3,
        minScale: startScale,
        startX,
        startY,
    });

    // console.log(panzoom.getOptions());
});

const handleWheel = (e: WheelEvent) => {
    panzoom.zoomWithWheel(e);
};

defineExpose({ mapWrapper, mapElement });
</script>

<template>
    <div ref="mapWrapper" class="map-wrapper" @wheel="handleWheel">
        <img
            ref="mapElement"
            src="../assets/map-customs.webp"
            alt=""
            class="map"
            :width="props.map?.width"
            :height="props.map?.height"
        />
    </div>
</template>

<style scoped lang="scss">
.map-wrapper {
    // position: absolute;
    inset: 1rem;
    border-radius: 1rem;
    background-color: rgba(0, 0, 0, 0.3);
    overflow: hidden;
}
.map {
    display: block;
}

.debug {
    position: absolute;
    bottom: 2rem;
    left: 2rem;
}
</style>
