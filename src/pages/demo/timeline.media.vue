<script setup>
import Moveable from "vue3-moveable";
import Selecto from "vue3-selecto";
import { ref } from "vue";

const windowRef = window

const moveableRef = ref(null);
const targets = ref([]);

const draggable = true;
const throttleDrag = 1;
const edgeDraggable = false;
const startDragRotate = 0;
const throttleDragRotate = 0;
const scalable = true;
const keepRatio = false;
const throttleScale = 0;
const snappable = true;
const bounds = { "left": 0, "top": 0, "right": 0, "bottom": 0, "position": "css" };
const targetRef = ref(null);
const onDrag = e => {
    e.target.style.transform = e.transform;
};
const onScale = e => {
    e.target.style.transform = e.drag.transform;
};
const onBound = e => {
    console.log(e);
};
const onDragStart = e => {
    const moveable = moveableRef.value;
    const target = e.inputEvent.target;
    if (target.tagName === "BUTTON" || moveable.isMoveableElement(target)
        || targets.value.some(t => t === target || t.contains(target))
    ) {
        e.stop();
    }
};
const onSelectEnd = e => {
    console.log(e)
    const moveable = moveableRef.value;
    if (e.isDragStart) {
        e.inputEvent.preventDefault();
        moveable.waitToChangeTarget().then(() => {
            moveable.dragStart(e.inputEvent);
        });
    }
    targets.value = e.selected;
};


</script>
<template>
    <div class="root">
        <div class="container" style="width: 500px;height: 100px;border: 1px solid #ccc;">
            <div class="target target1" style="left: 50px;top: 0;">Target1</div>
            <div class="target target2" style="left: 250px;top: 0;">Target2</div>
            <div class="target target3" style="left: 400px;top: 0;">Target3</div>
            <Moveable ref="moveableRef" :target="targets" :draggable="draggable" :throttleDrag="throttleDrag"
                :edgeDraggable="edgeDraggable" :startDragRotate="startDragRotate"
                :throttleDragRotate="throttleDragRotate" :scalable="scalable" :keepRatio="keepRatio"
                :throttleScale="throttleScale" :snappable="snappable" :bounds="bounds" @drag="onDrag" @scale="onScale"
                @bound="onBound" />
        </div>

        <Selecto :dragContainer="windowRef" :selectableTargets="['.target']" :hitRate="0" :selectByClick="true"
            :selectFromInside="false" :toggleContinueSelect="['shift']" :ratio="0" @dragStart="onDragStart"
            @selectEnd="onSelectEnd"></Selecto>
    </div>
</template>

<style>
.margin .root {
    position: static;
}

.description {
    padding: 10px;
}

.root {
    position: relative;
}

.container {
    position: relative;
    margin-top: 50px;
}

.will-change-container {
    padding-left: 100px;
    padding-top: 100px;
    will-change: transform;
}

.will-change-target {
    position: relative;
    width: 100px;
    height: 100px;
    line-height: 100px;
    text-align: center;
    background: #ee8;
    color: #333;
    font-weight: bold;
    border: 1px solid #333;
    box-sizing: border-box;
}

.target {
    position: absolute;
    width: 100px;
    height: 100px;
    top: 150px;
    left: 100px;
    line-height: 100px;
    text-align: center;
    background: #ee8;
    color: #333;
    font-weight: bold;
    border: 1px solid #333;
    box-sizing: border-box;
}

.target1 {
    left: 120px;
    top: 120px;
}

.target2 {
    left: 300px;
    top: 140px;
}

.target3 {
    left: 180px;
    top: 250px;
}

.nested {
    position: absolute;
    border: 4px solid #ccc;
    width: 100px;
    height: 100px;
    top: 50px;
    left: 70px;
    color: #333;
    /* box-sizing: border-box; */
}

.nested.first {
    top: 150px;
}

.nested.rotate {
    transform-origin: 0 0;
    transform: rotate(-30deg);
}

.nested.scale {
    transform: scale(1.5, 1.5);
}

.nested .target {
    top: 50px;
    left: 50px
}


/* pos guidelines */
.moveable-normal.red {
    background: red !important;
}

/* gap guidelines */
.moveable-gap.red {
    background: red !important;
}

/* When snapped to an element in elementGuidelines */
.moveable-bold.red {
    background: red !important;
}

/* A dashed line between target and element */
.moveable-dashed.red {
    border-top-color: red !important;
    border-left-color: red !important;
}

/* pos guidelines */
.moveable-normal.green {
    background: green !important;
}

/* gap guidelines */
.moveable-gap.green {
    background: green !important;
}

/* When snapped to an element in elementGuidelines */
.moveable-bold.green {
    background: green !important;
}

/* A dashed line between target and element */
.moveable-dashed.green {
    border-top-color: green !important;
    border-left-color: green !important;
}

.scrollArea {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: calc(100vh - 100px);
    overflow: auto;
}

.scrollArea:before {
    content: "";
    display: block;
    position: absolute;
    top: 0;
    left: 0;
    height: 300%;
    width: 100%;
    background: linear-gradient(#333,
            #fff);
}

.infinite-viewer {
    height: 500px;
}

.control-padding .moveable-around-control {
    background: #f55 !important;
}


.cube {
    display: inline-block;
    border-radius: 5px;
    width: 40px;
    height: 40px;
    margin: 4px;
    background: #eee;
    --color: #4af;
    color: #333;
    line-height: 40px;
    text-align: center;
}

.cube .cube {
    background: #ddd;
    margin-left: 20px;
}
</style>