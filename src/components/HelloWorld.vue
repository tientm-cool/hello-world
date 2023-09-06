<script setup>
import { onMounted, onUnmounted, reactive } from "vue"

let text = "Hello World"
const characterList = text.split("").map(character => {
    return {
        content: character,
        x: Math.floor(Math.random() * 1000),
        y: Math.floor(Math.random() * 1000)
    }
})
const state = reactive({
    characterList
})

let activeDragIndex = -1;
let mouseX = 0;
let mouseY = 0;
let elementX = 0;
let elementY = 0;
const onMouseDown = (event, index) => {
    event.preventDefault()
    activeDragIndex = index
    mouseX = event.clientX
    mouseY = event.clientY
    elementX = characterList[activeDragIndex].x
    elementY = characterList[activeDragIndex].y
}

const onMouseMove = () => {
    if (activeDragIndex >= 0) {
        state.characterList[activeDragIndex].x = elementX + event.clientX - mouseX
        state.characterList[activeDragIndex].y = elementY + event.clientY - mouseY
    }
}

let onMouseUp = () => {
    activeDragIndex = -1
}

onMounted(() => {
    window.addEventListener("mousemove", onMouseMove)
    window.addEventListener("mouseup", onMouseUp)
})

onUnmounted(() => {
    window.removeEventListener("mousemove", onMouseMove)
    window.removeEventListener("mouseup", onMouseUp)
})

</script>

<template>
    <div class="container">
        <div
            v-for="character in state.characterList"
            class="character-box"
            :class="{space: character.content === ' '}"
        ></div>
    </div>
    <template v-for="(character, i) in state.characterList">
        <div
            v-if="character.content !== ' '"
            class="character-item"
            :style="{
                top: character.y + 'px',
                left: character.x + 'px'
            }"
            @mousedown="onMouseDown($event, i)"
        >
            {{ character.content }}
        </div>
    </template>
</template>

<style scoped lang="scss">
.container {
    display: flex;
}

.character-box {
    border: 1px solid #ccc;
    border-radius: 5px;
    width: 60px;
    height: 90px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-right: 10px;

    &.space {
        border: none;
    }
}

.character-item {
    background: blue;
    color: #fff;
    border-radius: 5px;
    padding: 5px;
    width: 40px;
    height: 60px;
    font-size: 2em;
    position: absolute;
    display: flex;
    align-items: center;
    justify-content: center;
    user-select: none;
    cursor: grab;
}
</style>
