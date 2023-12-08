<template>
    <div style="position: absolute; "></div>
    <div style="position: absolute; float: left; z-index: 100;">
        <a-form :model="formState" :label-col="labelCol" :wrapper-col="wrapperCol">
            <a-form-item label="heading">
                <a-slider id="test" :max="360" :step="0.1" v-model:value="formState.heading" @change="onHeadingChange" />
            </a-form-item>
            <a-form-item label="Activity zone">
                <a-select v-model:value="formState.region" placeholder="please select your zone">
                    <a-select-option value="shanghai">Zone one</a-select-option>
                    <a-select-option value="beijing">Zone two</a-select-option>
                </a-select>
            </a-form-item>
            <a-form-item label="Activity time">
                <a-date-picker v-model:value="formState.date1" show-time type="date" placeholder="Pick a date"
                    style="width: 100%" />
            </a-form-item>
        </a-form>
    </div>
    <div ref="cesiumContainer" style="width: 100%;"></div>
    
</template>

<script setup>
import { ref, onMounted, defineComponent, reactive, toRaw } from 'vue';
import * as Cesium from "cesium";
import "cesium/Build/Cesium/Widgets/widgets.css";

const formState = reactive({
    name: '',
    region: undefined,
    date1: undefined,
    delivery: false,
    type: [],
    resource: '',
    desc: '',
    heading: undefined,
});

const cesiumContainer = ref()
window.CESIUM_BASE_URL = '/Cesium'

var viewer;
var camera;

const onHeadingChange = ()=>{
    camera.setView({
        destination: Cesium.Cartesian3.fromDegrees(-123.0744619, 44.0503000, 0.2),
        orientation: {
            heading: Cesium.Math.toRadians(formState.heading),
            pitch: Cesium.Math.toRadians(0),
            roll: Cesium.Math.toRadians(0)
        }
    });
}

onMounted(async () => {
    Cesium.Ion.defaultAccessToken = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiIxMzVkMmNjMi1hZmM3LTQ5YmEtYWUxMC1kZGI4ZGQ2MmQyMTYiLCJpZCI6MTgyNTgwLCJpYXQiOjE3MDE3NjU4NjB9.9B8RBQW__XK22qbDtKnwT4fmy6XBWu08Zk3fEx0dPzc'
    
    viewer = new Cesium.Viewer(cesiumContainer.value);

    camera = viewer.camera;
    camera.setView({
        destination: Cesium.Cartesian3.fromDegrees(-123.0744619, 44.0503000, 0.2),
        orientation: {
            heading: Cesium.Math.toRadians(0),
            pitch: Cesium.Math.toRadians(0),
            roll: Cesium.Math.toRadians(0)
        }
    });

    const position = Cesium.Cartesian3.fromDegrees(-123.0744619, 44.0503706, 0);
    const headingPositionRoll = new Cesium.HeadingPitchRoll();
    const fixedFrameTransform = Cesium.Transforms.localFrameToFixedFrameGenerator(
        "north",
        "west"
    );
    try {
        const model = await Cesium.Model.fromGltfAsync({
            url: "/models/a.glb",
            modelMatrix: Cesium.Transforms.headingPitchRollToFixedFrame(
                position,
                headingPositionRoll,
                Cesium.Ellipsoid.WGS84,
                fixedFrameTransform
            ),
            minimumPixelSize: 128,
        });
        viewer.scene.primitives.add(model);
    } catch (error) {
        console.log(`Failed to load model. ${error}`);
    }
})

</script>

