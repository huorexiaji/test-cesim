<template>
    <div ref="cesiumContainer"></div>
</template>

<script setup>
import { ref, onMounted  } from 'vue';
import { Viewer, Ion } from "cesium";
import "cesium/Build/Cesium/Widgets/widgets.css";

const cesiumContainer = ref()
window.CESIUM_BASE_URL = '/Cesium'

onMounted(()=>{
    Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiIxMzVkMmNjMi1hZmM3LTQ5YmEtYWUxMC1kZGI4ZGQ2MmQyMTYiLCJpZCI6MTgyNTgwLCJpYXQiOjE3MDE3NjU4NjB9.9B8RBQW__XK22qbDtKnwT4fmy6XBWu08Zk3fEx0dPzc'
    const viewer = new Viewer(cesiumContainer.value);
})

</script>

