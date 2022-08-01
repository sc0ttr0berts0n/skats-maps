<script setup lang="ts">
import { ref, Ref, onMounted, watch } from 'vue';
import Panzoom, { PanzoomObject } from '@panzoom/panzoom';
import { IMap } from '../App.vue';

const props = defineProps({
    map: Object,
});

const mapWrapper = ref() as Ref<HTMLElement>;
const mapElement = ref() as Ref<HTMLElement>;
let panzoom: PanzoomObject;

const scaleAndCenterMap = () => {
    const padding = 16;
    const bounds = mapWrapper.value.getBoundingClientRect();
    const map = {
        width: props.map?.width ?? 0,
        height: props.map?.height ?? 0,
    };
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
};

onMounted(scaleAndCenterMap);

watch(() => props.map, scaleAndCenterMap);

const handleWheel = (e: WheelEvent) => {
    panzoom.zoomWithWheel(e);
};

defineExpose({ mapWrapper, mapElement });
</script>

<template>
    <div ref="mapWrapper" class="map-wrapper" @wheel="handleWheel">
        <img
            ref="mapElement"
            :src="props.map?.url"
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
    background: repeating-linear-gradient(
        -45deg,
        rgba(26, 26, 26, 30%),
        rgba(26, 26, 26, 30%) 10px,
        rgba(13, 13, 13, 30%) 10px,
        rgba(13, 13, 13, 30%) 20px
    );
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
